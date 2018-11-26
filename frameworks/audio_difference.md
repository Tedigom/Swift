# AVAudioRecorder, AVAudioPlayer vs AVAudioEngine #

## 차이점 ##

### AVAudioRecorder, AVAudioPlayer ###
**하나의 트랙**(하나의 input, 하나의 output)에 대하여 재생 / 녹음할 경우에는 AVAudioPlayer, AVAudioRecorder를 사용합니다.

### AVAudioEngine ###

더 복잡한 오디오 처리를 위해서라면 AVAudioEngine을 사용합니다.

**AVAuidoInputNode** : 오디오의 입력

**AVAudioOutputNoted** : 오디오의 출력

가령, **여러 트랙**(여러 input들과 여러 output들)으로 녹음/ 재생을 하며, 노드들을 연결하여 섞인 트랙을 만들기도 하고, 오디오 스트림들 처리, 3D 오디오와 같은 이팩트를 넣는 작업도 합니다.
