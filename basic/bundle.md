# The App Bundle

iOS 앱을 빌드하게 되면 Xcode는 앱을 bundle로 묶어주는데, bundle은 앱과 연관된 리소스들을 모아놓은 디렉토리입니다. Bundle에는 앱 실행 파일과 supporting resources(앱 아이콘, 이미지 파일, 현지화 파일..)이 들어있습니다.

## The Information Property List File (info.plist)

Xcode는 컴파일하는 동안 프로젝트의 General, Capabilities, Info tab의 정보들을 가지고 Info.plist를 생성합니다.

모든 앱은 info.plist를 가지고 있어야 합니다. 기본값이 포함되어 있지만 위에서의 경우처럼 대부분이 변경이나 추가가 필수적입니다. 탭을 통해 변경 및 추가를 하거나 직접 source에서 변경 및 추가를 할 수 있습니다. 때문에 새 프로젝트를 시작할 때 어떤 기능이 필요한지를 잘 고려해서 키를 추가해줘야 합니다.

## Declaring the Required Device Capabilities

모든 앱은 실행에 필요한 장치별 기능을 선언해야 합니다.
Array를 이용해서 키의 값을 지정하는 경우, 키가 있으면 해당 기능이 필요함을 나타내고, 키가 없으면 기능이 필요하지 않음을 나타내며, 없어도 애플리케이션이 실행됩니다.
Dictionary를 이용해서 키의 값을 지정하는 경우, Boolean값을 이용해서 표시하는데 true값은 필수 기능을 나타내고 false값은 디바이스에 기능이 없음을 나타냅니다. 앱에서 특정 기능을 선택할 수 있는 경우 Dictionary에 해당 키를 포함하면 안 됩니다.

## App Icons

앱 아이콘은 Image Assets에 포함되어 있어야 합니다. 자세한 내용은 Apple의 iOS Human Interface Guidelines에 나와있습니다.

## App Launch (Default) Images

앱을 실행시켰을 때 잠깐 보이는 정적인 이미지로, 앱 화면을 구성하고 사용자에게 보여질 준비가 완료되면 사라집니다. 스냅샷을 사용할 수 있을 경우, launch image가 아닌 스냅샷을 이용하게 됩니다.

앱이 foreground(현재 실행 상태)에서 background로 들어갈 때는 스냅 샷이 생성되며 다시 foreground로 돌아가면 런칭 이미지가 아닌 스냅 샷을 활용합니다.

앱을 오랫동안 실행하지 않은 경우에는 스냅 샷을 삭제하고 기존의 런칭 이미지를 활용하게 됩니다.
