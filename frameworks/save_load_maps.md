# Saving and Loading Maps #


## ARWorldMap ##

간단히 말하자면, world-tracking AR 세션으로부터의 anchor들과 공간이 매핑되어있는 상태를 의미합니다.


world map의 세션 상태에는 유저가 ARKit의 기기를 움직힌 물리적 공간에 대한 인식 뿐만 아니라, 세션에 더해진 ARAnchor 객체들 또한 포함합니다.

**getCurrentWorldMap(completionHandler:)** 를 호출하여, 세션의 worldmap을 저장한 후, configuration의 initialWorldMap property에 저장한 worldmap을 할당할 수 있으며, **run(_:options)** 을 호출하여, 다른 세션에서 동일한 것들에 대하여 볼 수 있습니다.

아래는 샘플 코드입니다.

```swift
// Saving and Loading World Maps
// Retrieve world map from session object
session.getCurrentWorldMap { worldMap, error in
	guard let worldMap = worldMap else {
		showAlert(error)
		return
	}
}

// Load world map and run the configuration
let configuration = ARWorldTrackingConfiguration()

configuration.initialWorldMap = worldMap

session.run(configuration)

```

이렇게 worldmap들을 저장하고, 그것들을 이용하여 새로운 세션을 시작함으로써, 앱은 새로운 AR적 능력들을 사용할 수 있습니다.

### *Multiuser AR experiences* ###

 아카이브된 ARWorldMap 객체들을 근처 유저의 기기에 보냄으로써, 공유된 reference 프레임을 만들 수 있습니다. 두 기기가 동일한 worldmap을 추적하면, 두 유저들은 동일한 가상 content를 보고 상호 작용할 수 있는 네트워크 환경을 구축 할 수 있습니다.

### *Persistent AR experiences* ###

 앱이 비활성화 상태일 때 world map을 저장하고, 앱이 다시 동일한 물리적 환경에서 시작하였을 때 복구합니다. 다시 실행된 world map의 anchor들을 이용하여, 동일한 가상의 content를 저장된 세션의 동일한 위치에 놓게 이용할 수 있습니다.


## Archiving World Map Data for Persistence or Sharing ##

요약 : 네트워크 전송 혹은 파일 저장을 위한 world map 객체들의 **Serialize**와 **deserialize**.

ARWorld 클래스는 **NSSecureCoding** 프로토콜을 따르기때문에, **NSKeyedArchiver**와 **NSKeyedUnarchiver** 클래스들을 이용하여, world map을 바이너리 데이터 형태로 변환/ 바이너리 데이터 형태로부터 변환 을 할 수 있습니다.

ARWorldMap을 file URL에 저장하는 방법

```swift
func writeWorldMap(_ worldMap: ARWorldMap, to url: URL) throws {
    let data = try NSKeyedArchiver.archivedData(withRootObject: worldMap, requiringSecureCoding: true)
    try data.write(to: url)
}

```

file URL로부터 ARWorldMap을 로드하는 방법

```swift
func loadWorldMap(from url: URL) throws -> ARWorldMap {
    let mapData = try Data(contentsOf: url)
    guard let worldMap = try NSKeyedUnarchiver.unarchivedObject(ofClass: ARWorldMap.self, from: mapData)
        else { throw ARError(.invalidWorldMap) }
    return worldMap
}
```


ARWorldMap을 다른 기기로 보내는 방법 : 다중 사용자 AR에 대한 reference 공유 프레임을 만듭니다.

1. 하나의 기기에서 **NSKeyedArchiver**를 이용하여, world map을 데이터 객체로 변환합니다.(네트워크를 통한 전송을 위해 만든 데이터를 파일에 쓸 필요는 없습니다.)
2. 네트워킹 기술 중 어느 것이든 선택하여, 다른 기기에 보냅니다. (문서에서는 **MultipeerConnectiviy** session을 이용하여, 전송하였습니다.)
3. 받는 기기에서는 **NSKeyedUnarchiver**를 이용하여, 데이터로부터 ARWorldMap을 만듭니다.


## worldMappingStatus ##

 해당 프레임에서 world map을 생성 혹은 relocalizing의 실현 가능성 나타내는 property.

 world-tracking 세션은 사용자의 환경에서의 기기의 위치를 결정하는 world map을 내부적으로 빌드합니다.

 세션 동안에 **getCurrentWorldMapWithCompletionHandler:** 를 언제든 호출할 수 있는 데, worldmap을 가져오는게 항상 일정하게 좋게 가져오지 않는다. 이 property를 사용하여, 세션이 ARWolrdMap을 생성하기에 충분한 데이터를 가졌는지 아닌지를 판단하여, 결정을 하면 좋을 것 같습니다.




| MappingStatus | Description |
|:--------|:--------|
|**ARWorldMappingStatusNotAvailable** | world map이 이용 가능하지 않습니다. |
|**ARWorldMappingStatusLimited** | world tracking이 현재 기기 위치 주변을 아직 충분히 매핑하지 않았습니다.|
|**ARWorldMappingStatusExtending** | world tracking이 최근 방문한 지역들을 매핑하였지만, 현재 기기 위치 주변을 아직 매핑 중인 상태입니다.|
|**ARWorldMappingStatusMapped** | world tracking이 보이는 장소에 대하여 충분히 매핑하였습니다.|
