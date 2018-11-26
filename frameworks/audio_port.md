# AVAudioSession Port Type 정리 #

이 문서는 애플 문서의 [InputPort](https://developer.apple.com/documentation/avfoundation/avaudiosessionportdescription/input_port_types), [OutputPort](https://developer.apple.com/documentation/avfoundation/avaudiosessionportdescription/output_port_types), [I/OPort](https://developer.apple.com/documentation/avfoundation/avaudiosessionportdescription/i_o_port_types)를 참고 하였습니다.

## Input Port Types ##

### lineIn ###

*Line-level input from the dock connector*

- **Line-level**이란? [설명 링크](#https://www.jh-studio.com/archives/306) 잘 번역이 되어있는 것같아 같이 올립니다.

	- 일반적인 유선마이크나 무선 마이크의 경우에는 mic-level이며, 대부분의 오디오 기기들이 사용하는 것이 '라인 레벨'입니다.

- 즉, line-level에 대한 input이 들어 온 경우에 **lineIn**이 호출이 됩니다.


### builtInMic ###

*The built-in microphone on a device*

기본 아이폰 등의 기기에 들어있는 내장 마이크.


### headsetMic ###

*A microphone that is built-in to a wired headset*

유선 이어폰의 마이크(있는 경우). 코드상으로는 **MicrophoneWired** 라고 호출이 됩니다.


## Output Port Types ##

### lineOut ###

*Line-level output to the dock connector*

**lineOut** 단자란? 기본적으로 해당 소스를 재증폭하여, 출력하는 기기에 연결하는 용도.

위에서 정의한 것처럼 라인 레벨에서의 출력을 해주는 port type을 말함.

### headphones ###

*Output to a wired headset*

기본적인 유선 이어폰.

### builtInReceiver ###

*Output to a speaker intended to be held near the ear*

일반적인 상황에서의 기기의 사운드(외부로 나가는 소리가 아님).

### builtInSpeaker ###

*Output to the device’s built-in speaker*

통화시 등에서 외부 스피커 모드로 변경 등이 이에 해당한다. 외부로 그게 들리게 하기 위한 기기 내의 외부 스피커를 사용했을 경우에 사용.

### HDMI ###

*Output to a device via the High-Definition Multimedia Interface (HDMI) specification*

HDMI 포트를 통하여 나가는 포트.


### airPlay ###

*Output to a remote device over AirPlay*

AirPlay를 통하여 오디오를 애플 TV, 맥, AirPlay 호환 스피커 등으로 송출하는 Port.



### bluetoothLE ###

*Output to a Bluetooth Low Energy (LE) peripheral*

저전력 블루투스인 주변 기기의 스피커 등의 Port.

[Accessory Design Guidelines for Apple Devices](https://developer.apple.com/accessories/Accessory-Design-Guidelines.pdf) 이 가이드에서 애플 기기에서의 블루투스 등의 기기에 대한 가이드라인을 보여주고 있습니다.


### bluetoothA2DP ###

*Output to a Bluetooth A2DP device*

블루투스는 프로파일이라고 미리 정의된 프로토콜을 통해 통신해야함.

스테레오 오디오 프로파일(**A2DP**) : 스테레오 출력에 사용되는 프로파일. 블루투스로 노래를 들을 때에 보통 사용. **소리 출력만 전송, 마이크 사용할 수 없다**


## I/O Port Types ##

### bluetoothHFP ###

*Input or output on a Bluetooth Hands-Free Profile device*

Hands Free(헤드셋) 프로파일(**HFP**) : 통화 목적 프로파일이며, 모노 사운드와 마이크데이터를 주고 받는 프로파일.


참고 - 블루투스 헤드셋 -> A2DP, HFP로 연결하여, 노래 듣다가 전화 변경 가능!, 하지만, 스테레오 사운드(A2DP)와 마이크는 동시 작동 불가.


### usbAudio ###

*Input or output on a Universal Serial Bus device*

USB 오디오에 연결하여 오디오 신호 전송하여 입출력하는 Port.

### carAudio ###

*Input or output via Car Audio*

이 부분은 아직 작성중입니다.

아이폰이 직접적으로 자동차에 연결시 CarPlay가 실행 될 경우의 Port로 알고있습니다.

**이슈** : 그렇다면 블루투스로 CarPlay를 연결한다, HFP 포트가 될지? 좀 더 알아봐야할듯합니다.
