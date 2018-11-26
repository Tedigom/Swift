# Application Life Cycle

 iOS 앱이 실행되서 실행되는 동안의 과정을 Application Life Cycle 이라고 부릅니다. 일반적으로 C언어 기반의 프로그래밍 언어에서는 main이라는 함수가 앱의 시작이 됩니다. iOS의 앱 또한 ObjectiveC 기반에서(C언어 기반) 돌아가기 때문에 앱은 main 함수에서 시작합니다. 다만, iOS의 핵심 라이브러리인 UIKit framework가 main 함수를 관리하여 앱 개발자들이 직접 main 함수에 코드를 작성하지 않습니다.

그렇다고 앱의 실행에 앱 개발자가 관여할 수 없는 것은 아닙니다. UIKit은 main 함수를 다루는 과정에서 UIApplicationMain 함수를 실행합니다. 이 함수를 통해 UIApplication 객체가 생성되는데 이 객체를 통해 앱 개발자는 앱의 실행에 부분적으로 관여할 수 있습니다. 이처럼, 앱 개발자가 앱을 실행할 때 접근할 수 객체가 UIApplication 이기 때문에 앱이 어떤 과정으로 실행되는지 자세히 알아보려면 UIApplication에 대해 좀 더 알 필요가 있습니다.

> iOS도 C언어 기반의 언어로 만들어졌기 때문에, main 함수가 존재합니다. 이 main 함수는 앱의 시작점이 됩니다. 다만, main 함수는 숨겨져 있기 때문에 프로젝트를 생성해도 파일이 나타나지는 않습니다.

## UIApplication

모든 iOS 앱들은 UIApplicationMain 함수를 실행합니다. 이 때 생성되는 것 중 하나가 UIApplication 객체입니다. UIApplication 객체는 singleton 형태로 생성되어, UIApplication.shared의 형태로 앱 전역에서 사용할 수 있습니다. UIApplication 객체의 가장 중요한 역할은 user의 이벤트(터치, 리모트 컨트롤, 가속도계, 자이로스코프 등)에 반응하여 앱의 초기 routing(초기 설정)을 하는 것입니다. 구체적인 예를 들자면, UIApplication 객체는 앱이 Background에 진입한 상태에서 추가적인 작업을 할 수 있도록 만들어주거나, 푸쉬 알람을 받았을 때 어떤 방식으로 이를 처리할지 등에 대한 것을 다룹니다.

## Main Run Loop

Main Run Loop라는 것은 유저가 일으키는 이벤트들을 처리하는 프로세스입니다. UIApplication 객체는 앱이 실행될 때, Main Run Loop를 실행하고, 이 Main Run Loop를 View와 관련된 이벤트나 View의 업데이트에 활용합니다. 또한, Main Run Loop는 View와 관련되어 있기 때문에 Main 쓰레드에서 실행됩니다.

<img src = "https://dl.dropbox.com/s/i6ed655jlzrizs1/IMG_1006.PNG" width = 600>

유저가 일으키는 이벤트의 처리 과정을 다음과 같은 순서로 정리할 수 있습니다.

- 1. 유저가 이벤트를 일으킨다.
- 2. 시스템을 통해 이벤트가 생성된다.
- 3. UIkit 프레임워크를 통해 생성된 port로 해당 이벤트가 앱으로 전달된다.
- 4. 이벤트는 앱 내부적으로 Queue의 형태로 정리되고, Main Run Loop에 하나씩 매핑된다.
- 5. UIApplication 객체는 이때 가장 먼저 이벤트를 받는 객체로 어떤 것이 실행되야하는지 결정한다.



### Reference:
- https://hcn1519.github.io/articles/2017-09/ios_app_lifeCycle
