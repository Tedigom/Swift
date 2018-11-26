# Output과 Input을 자신이 원하는 포트로 나오게 하기 #

## Input ##

가능한 input들을 찾아, setPreferredInput으로 자신이 원하는 Input을 바꿔주면 됩니다.

### AVAudioSession.availableInputs ###

availableInputs는 현재 연결된 Input들([AVAudioSessionPortDescription]?)을 보여주는 property입니다.

### preferredInput, setPreferredInput ###

- preferredInput : preferred된 입력포트를 반환합니다.
 - 기존에 preferred된 입력포트 존재한다면, 해당 입력포트를 반환
 - 기존에 preferred된 입력포트가 없을 경우에는 nil을 반환

- setPreferredInput : 이 메소드는 선택한 입력 포트를 사용하는 입력 포트로 사용하게 합니다.
  - 사용자가 사용하고 있는 입력 포트와 동일한 입력 포트일 경우 :  아무런 영향이 없음.
  - 다른 입력 포트일 경우에, 입력 포트를 바꿔줌.
  - nil 값을 파라미터로 넣어 초기화


 아래는 관련 코드입니다.

```swift
do {
      try audioSession.setPreferredInput(input)
} catch {
      print("setpreferredInput error \(error)")
}

```


## Output ##

### Loud Speaker ###

기본 아이폰의 스피커는 기존에 테스트한 결과 작게 나오는데, 그 이유가 Loud Speaker를 설정을 해주지 않아서 였습니다. 아래는 Loud Speaker를 설정하는 코드입니다.

```swift
do {
            try audioSession.overrideOutputAudioPort(AVAudioSessionPortOverride.speaker)
            } catch {
                print("speaker port override error \(error)")
            }
```

여기서 AVAudioSessionPortOverride.speaker의 경우를 AVAudioSessionPortOverride.none이라고 바꿀 경우에는, 현재 오디오의 카테고리의 default에 해당하는 오디오 route를 리턴해줍니다. 즉, Speaker가 아닌 현재의 output으로 output을 선택하려면, 아래와 같이 AVAudioSessionPortOverride를 .none이라고 설정 해주면 됩니다.


```swift
do {
            try audioSession.overrideOutputAudioPort(AVAudioSessionPortOverride.speaker)
            } catch {
                print("speaker port override error \(error)")
            }
```
