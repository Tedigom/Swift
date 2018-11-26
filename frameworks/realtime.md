# Using CADisplayLink & AVPlayerItemVideoOutput

CADisplayLink는 applicationdl drawing을 디스플레이의 새로 고침 빈도와 동기화 할 수있게 해주는 타이머 객체입니다. CADisplayLink의 인스턴스를 초기화 시 타겟 객체와 셀렉터를 지정할 수 있고, 디스플레이 루프를 디스플레이와 동기화하기 위해 add (for : forMode : ) 메소드를 사용하여 실행 루프에 추가합니다.

CADisplayLink와 실행 루프가 연결되면 화면의 내용을 업데이트해야 할 때 타겟의 셀렉터가 호출됩니다. 디스플레이 링크가 실행 루프와 연결되면 화면의 내용을 업데이트해야 할 때 대상의 셀렉터가 호출됩니다. 대상은 디스플레이 링크의 타임스탬프 속성을 읽어 이전 프레임이 표시된 시간을 검색할 수 있습니다.

```Swift
displayLink = CADisplayLink(target: self, selector: #selector(displayLinkDidRefresh(link:)))

displayLink?.add(to: RunLoop.main, forMode: RunLoopMode.commonModes)
```

AVPlayerItemVideoOutput을 사용하면 코어 비디오 픽셀 버퍼와 관련된 내용의 출력을 조정할 수 있습니다.

```swift
@objc func displayLinkDidRefresh(link: CADisplayLink) {
        let itemTime = videoOutput.itemTime(forHostTime: CACurrentMediaTime())

        if videoOutput.hasNewPixelBuffer(forItemTime: itemTime) {
            if let pixelBuffer = videoOutput.copyPixelBuffer(forItemTime: itemTime, itemTimeForDisplay: nil) {

               // do something with pixelBuffer
            }
        }
    }
```

기본 설정 FramesPerSecond를 설정하여 디스플레이 링크의 프레임률을 제어할 수 있습니다. 그러나 실제 초당 프레임 수는 사용자가 설정한 기본 값과 다를 수 있습니다. 실제 프레임률은 항상 장치의 최대 새로 고침 빈도를 나타내는 요소입니다.

예를 들어 장치의 최대 새로 고침 빈도가 초당 60 프레임(maximumFramesPerSecond로 정의됨)인 경우 실제 프레임률에는 초당 15, 20, 30 및 60 프레임이 포함됩니다. 디스플레이 링크의 기본 프레임률을 최대값보다 높은 값으로 설정하면 실제 프레임률이 최대값입니다.

최대 프레임률의 점수가 아닌 선호 프레임률은 가장 가까운 인수로 반올림됩니다. 예를 들어 초당 최대 새로 고침 빈도가 60인 장치에서 기본 프레임률을 초당 26 또는 35 프레임으로 설정하면 실제 프레임률이 초당 30회 생성됩니다.

preferredFramesPerSecond 값이 0이면 기본 설정 프레임률이 maximumFramesPerSecond 속성으로 표시되는 디스플레이 최대 새로 고침 빈도와 동일합니다.

응용 프로그램이 디스플레이 링크와 함께 완료되면 invalidate()를 호출하여 모든 런 루프에서 제거하고 대상과 연결을 끊어야 합니다

```Swift
displayLink?.remove(from: RunLoop.main, forMode: RunLoopMode.commonModes)
```
