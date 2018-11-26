
# Video

## AVAsset
AVAsset은 **비디오 및 사운드와 같은 시각화 된 오디오 비주얼 미디어를 모델링하는데 사용되는 추상 불변 클래스**입니다. AVAsset은 Asset을 구성하는 트랙의 속성을 정의합니다. Asset에는 오디오, 비디오, 텍스트, 자막 등 단일 미디어 유형의 각 트랙을 함께 표시하거나 처리 할 수있는 하나 이상의 트랙이 포함될 수 있습니다.

## AVPlayer
AVPlayer는 **미디어 Asset의 재생 및 타이밍을 관리하는 데 사용되는 컨트롤러 개체**입니다. 즉, AVPlayer는 미디어의 타임 라인 내에서 **재생, 일시 정지, 재생 속도 변경, 다양한 시점 탐색과 같은 플레이어의 전송 동작을 제어하는 ​​인터페이스를 제공**합니다. AVPlayer를 사용하여 QuickTime 동영상 및 MP3 오디오 파일과 같은 로컬 및 원격 파일 기반 미디어뿐만 아니라 HTTP 라이브 스트리밍을 사용하여 시청각 미디어를 재생할 수 있습니다.

## AVPlayerLayer
AVPlayerLayer는 **AVPlayer 객체가 시각적 출력을 지시 할 수있는 CALayer의 하위 클래스**입니다. AVPlayerView 및 AVPlayerViewController와 달리, **재생 컨트롤을 제공하지 않고 단순히 화면에 시각적 내용을 표시**합니다(커스텀에 적합). UIView의 백업 레이어로 사용하거나 수동으로 레이어 계층 구조에 추가하여 비디오 내용을 화면에 표시 할 수 있습니다.

## CMTime
AVFoundation에서는 미디어의 시간을 다룰 때 CMTime을 사용합니다. CMTime은 **분자 (int64_t value)와 분모 (int32_t timescale)가 있는 유리수로 표시**됩니다. 따라서 timescale이 4 인 경우 각 단위는 1/4 초를 나타냅니다. timescale이 10 일 경우 각 단위는 1/10 초를 나타냅니다.
