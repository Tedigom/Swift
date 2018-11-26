# States of Application

iOS 앱의 실행 전 상태, 혹은 실행 후 상태 등을 세분화하여 알아보고, 각각의 상태별로 접근하기 위한 방식에 대해 알아보고자 합니다.

## App State

앱의 상태라는 것은 여러가지 의미를 내포한 폭넓은 의미로 받아들여질 수 있습니다만, Apple에서 정의하는 앱의 상태(App State)는 크게 5가지로 구분됩니다.

<img src="https://dl.dropbox.com/s/wpmf59gfnaiuafr/IMG_1008.PNG" width=400>

- Not Running:

> 아무것도 실행하지 않은 상태

- InActive:

> 앱이 Foreground 상태로 돌아가지만, 이벤트는 받지 않는 상태, 앱의 상태 전환 과정에서 잠깐 머무는 단계입니다.

- Active:

> 일반적으로 앱이 돌아가는 상태

- Background:

> 앱이 Suspended(유예 상태) 상태로 진입하기 전 거치는 상태,

- Suspended:
> 앱이 Background 상태에 있지만, 아무 코드도 실행하지 않는 상태, 시스템이 임의로 Background 상태의 앱을 Suspended 상태로 만듭니다.


위의 상태에서 몇 가지 알아두면 좋은 점들이 있습니다.

> 1. Background 상태에서는 일부 필요한 추가 작업을 수행할 수 있습니다. 또한, Background 상태에서 앱을 실행하면 InActive 상태를 거치지 않고 앱이 실행됩니다.(iOS에서 홈버튼을 두 번 눌러서 앱을 전환할 때, 앱이 재시작되지 않는다면 해당 앱은 Background 상태에 있던 앱입니다.)

> 2. 앱이 죽는 것(Suspended 상태에서 Not Running 상태로 진입하는 것)에는 알림을 받을 수 없습니다. 또한 Background 상태에서 Suspended 상태로 진입할 때 willTerminate 메소드가 실행되지만 이 또한 기기를 재부팅하면 실행되지 않습니다.

> 3. iOS 앱의 다양한 상태가 있지만, 주요한 작업은 Active, Background 상태에서 주로 이뤄지게 됩니다.


## AppDelgate  

앱의 상태에 대해서 알아보았으니, 이번에는 이 상태에 접근하기 위한 방법에 대해 알아보고자 합니다. 이 때 각각의 상태에 접근하기 위해 사용되는 파일이 AppDelegate.swift입니다. iOS 앱 프로젝트를 생성하면 AppDelegate.swift은 자동으로 생성됩니다.

AppDelegate은 이름 그대로 앱과 시스템의 연결을 위해 필요한 delegate 메소드를 담고 있습니다. 다만, 이름 때문에 그런 것은 아니고, @UIApplicationMain이라는 annotation이 있기 때문에 앱에서 AppDelegate.swift을 앱과 시스템을 연결하기 위한 파일로 인식합니다. AppDelegate.swift의 코드를 살펴보면 다음과 같습니다.

```swift
// AppDelegate.swift
import UIKit

@UIApplicationMain
class AppDelegate: UIResponder, UIApplicationDelegate {

    var window: UIWindow?

    func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplicationLaunchOptionsKey: Any]?) -> Bool {
        return true
    }

    func applicationWillResignActive(_ application: UIApplication) {
    }

    func applicationDidEnterBackground(_ application: UIApplication) {
    }

    func applicationWillEnterForeground(_ application: UIApplication) {
    }

    func applicationDidBecomeActive(_ application: UIApplication) {
    }

    func applicationWillTerminate(_ application: UIApplication) {
    }
}
```
AppDelegate 객체는 UIResponder, UIApplicationDelegate을 상속 및 참조하고 있습니다. 먼저 UIResponder는 앱에서 발생하는 이벤트들을 담고 있는 추상형 인터페이스 객체로 View와 사용자의 이벤트간의 연결을 관리하는 역할을 합니다.

UIApplicationDelegate은 UIApplication 객체의 작업에 개발자가 접근할 수 있도록 하는 메소드들을 담고 있습니다. 예를 들어 설명하자면, didFinishLaunchingWithOptions, applicationWillResignActive 등과 같은 메소드를 통해 앱의 상태가 변할 때 수행할 작업들을 설정할 수 있습니다. UIApplicationDelegate을 통해서 우리는 앱의 상태(Foreground, Background, Suspended 등)가 변하는 순간에 따라 앱에서 어떤 작업을 수행할 것인지 결정할 수 있습니다.

### Reference:
- https://hcn1519.github.io/articles/2017-09/ios_app_lifeCycle
