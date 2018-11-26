
# SiriKit

## Overview

Siri는 **Intent**를 사용하여 유저에게 보여줄 구문과 동작을 형식화합니다. SiriKit은 사용자가 수행할 수 있는 Request 유형(Intents)을 정의합니다. Intent는 시리와 통신하는 데 사용되는 객체입니다.

각 Intent는 INIntent 하위 클래스로 표시되며, 이 클래스와 관련된 handler protocl 및 특정 INIntentResponse 하위 클래스가 SiriKit과 다시 대화할 수 있습니다.

Siri는 자신이 할 수 있는 일에 한계가 있고 알려진 도메인 내에서만 작동합니다. 도메인은 VoIP 통화, 메시지, 결제, 목록 및 메모 작업, 작업 지원 등 Siri가 알고 있는 사항의 카테고리입니다. 각 도메인에는 시리가 수행할 수 있는 일련의 Intent들이 있습니다.

그러나 **Siri Shortcut**을 위한 **Custom Intents**를 통해 앱에서 수행되는 작업과 관련된 Siri에 대한 요청 및 응답에 대한 자체 모델을 정의할 수 있습니다. 이러한 행동들은 애플의 System intent domain 에 의해 제한되지 않습니다.


SiriKit은 서비스를 Siri 및 Maps와 통합하는 app extensions을 구현하는 데 사용하는 Intents 및 Intents UI 프레임워크를 포함합니다. SiriKit는 두 가지 유형의 app extensions을 지원합니다.


> Intents app extensions는 SiriKit로부터 사용자 요청을 수신하고 이를 app-specific action으로 변환합니다. 예를 들어, 사용자는 Siri에게 메시지를 보내거나, 차를 예약하거나, 앱을 사용하여 운동을 시작하도록 요청할 수 있습니다.

> Intents UI app extensions는 사용자 요청을 충족하는 경우, Siri 또는 Maps 인터페이스에 브랜드 또는 기타 사용자 지정된 콘텐츠를 표시합니다. Intent를 처리할 때 Siri가 embeded view로 표시할 뷰 컨트롤러를 정의할 수 있습니다. 이 extension의 생성은 선택 사항입니다.


 <img src='https://docs-assets.developer.apple.com/published/63ea07d81b/16f9b205-2c79-4bea-b89a-2ef26bc71ef1.png'  width='500'>


Intent의 lifecycle은 3단계로 정의됩니다: Resolve, Confirm and Handle.

1. **Resolve**:

사용자가 제공한 값을 Siri가 이해하는 데 도움을 줍니다. Siri가 다음 결정을 내리는 데 도움이 되는 정보를 제공할 수 있으며, Siri에게 상호 작용이 성공적이었는지 또는 확인 또는 거부와 같은 추가 정보가 필요한지 알려줍니다.

2. **Confirm**:

제공된 Intent를 완료하기 위한 모든 요구 사항을 앱이 가지고 있는지 보고합니다. 이 단계에서는 Siri에게 예상되는 결과와 ,필요한 경우, 확인 요청을 알립니다.

3. **Handle**:

최종적이고 가장 중요한 단계입니다. 여기서 앱이 Intent에 대한 실제 처리를 수행하고 작업이 성공적으로 완료된 것을 보고합니다. 그러나 경우에 따라 오류가 발생할 수 있습니다. 또한 느린 네트워크 연결과 같은 요청을 완료하는 데 더 많은 시간이 필요한 경우 사용자가 Siri 상호 작용을 종료한 후에도 작업이 계속된다는 것을 Siri에게 알릴 수 있습니다.



 ## How to use  

### Phase 1: Setting

#### 1.1 Project -> Capabilities 탭 ->  Siri capability 항목 활성화

 <img src="https://docs-assets.developer.apple.com/published/381143513b/14586ba5-7db2-49b5-8104-0339d20f663a.png"  width='500'>

#### 1.2 Intents App Extension 추가

<img src="https://docs-assets.developer.apple.com/published/b0271f8ff5/bd80a652-5afd-448e-86c3-50941771d1fe.png"  width='500'>

Siri(시리)에서 오는 모든 Intent가 처리되고 Extension handler에 의해 해당 intent가 포함된 내용이 처리됩니다. Extension에는 어떤 항목이 지원되고 어떤 항목이 잠금 화면에 있는 동안 제한되는지 정의해야 하는 Info.plist가 포함되어 있습니다. Intents app extension은 Siri 및 Maps에서 시작된 사용자 요청에 대한 응답을 제공합니다.

#### 1.3 Custom intent를 위하여 a new Siri Intents Definition file (.intentdefinition 파일) 생성

<img src="https://images.ctfassets.net/3cttzl4i3k1h/5qczJWkuMEQIikWgOSiw6c/3b6fab786a9387077799760358c75ecd/image1.png"  width='500'>

Custom Siri Intent는 다음과 같은 두가지 파트로 이루어져 있습니다.

> 1. Intent : 사용자가 Siri에게 제공할 파라미터를 정의합니다.


> 2. Response : Siri가 요청을 처리한 후 의도에 응답하는 방법을 정의합니다. (Success / Failure 각각의 상황에 대응 )




#### 1.4 Specify the Intents that Your Extension Supports

> Xcode에서 Intents 앱 확장자의 Info.plist 파일을 선택 후, NSExtension 및 NSExtensionAttributes 키를 확장하여 IntentsSupported 키에서 확장에서 처리하는 각 **Supported Intents** 들을 추가합니다. 이외에도 IntentsRestrictedWhileLocked key 등을 설정할 수 있습니다.

> Intent UI를 사용하는 경우, 마찬가지로 Info.plist에서 NSExtension/NSExtensionAttributes/IntentsSupported 키를 확장하여 Supported Intents들을 추가합니다.


### Phase 2: Handling Intent


Siri가 사용자의 앱과 관련된 작업을 수행하려는 경우 Intents Extension이 로드됩니다. OS는 확장의 시작 지점으로 NSExtensionPrincipalClass를 찾는 확장의 Info.plist를 검사합니다. 이 클래스는 INExtension의 하위 클래스여야 하며 INIntentHandlerProviding 함수: 핸들러(for intent: INIntent) -> Any?를 구현해야 합니다.

```Swift
import Intents

class IntentHandler: INExtension /* list of `Handling` protocols conformed to */ {
    override func handler(for intent: INIntent) -> Any? {
      if intent is CustomIntent {
          return CustomIntentHandler()
      }
      return nil
    }
}
```
#### 2.1 Resolve

Resolve function의 목적은 매개변수를 해결하는 것입니다. 이 단계에서, extension은 그 목적에 필요한 모든 정보가 존재하는지 확인해야 합니다. 누락된 정보가 있으면 사용자에게 추가 질문을 할 수 있습니다.

매개변수가 필수인지 option인지, 유저로부더 향후 추가 인풋을 받는지 아닌지 등을 알아내는 것입니다. Resolve function은 매개변수가 주어지지 않은 가능성을 포함하여, 주어진 intent로부터 받은 데이터를 검사합니다. 주어진 intent 데이터에 따라 INIntentResolutionResult를 만들어 Siri에게 매개변수가 resolve 된 방법을 알려줍니다.

예를들어 ride request intent에 아래와 같은 매개변수가 있다고 가정하겠습니다.
- Pickup location
- Drop-off location
- Party size
- Ride option
- Payment method

각 매개변수는 handler protocol에 관련 메서드와 함께 제공됩니다. 이 앱에서 intent를 처리하기 위해 INRequestRideIntentHandling을 사용하고 있습니다. 이 프로토콜에는 위의 각 매개 변수를 해결하는 함수가 있습니다. 각각은 매개 변수로 ride request intent를 수신하고 completion block이 있으며, 이 블록은 해당 intent를 처리할 때 호출합니다. completion block은 INIntentResolutionResult 하위 클래스를 매개 변수로 사용합니다.

해결 결과는 Siri에게 다음에 무엇을 해야 하는지 알려주거나, 모든 것이 정상이면 다음 매개변수로 이동합니다.


아래는 INRequestRideIntent (Custom) 를 받아 party size 매개변수를 Resolve하여 INIntegerResolutionResult (Custom) 를 리턴하는 예시입니다.

```swift
func resolvePartySize(forRequestRide intent: INRequestRideIntent, with completion: @escaping (INIntegerResolutionResult) -> Void) {
  switch intent.partySize {
  case .none:
    completion(.needsValue())
  case let .some(p) where simulator.checkNumberOfPassengers(p):
    completion(.success(with: p))
  default:
    completion(.unsupported())
  }
}
```



#### 2.2 Confirm

모든 매개변수가 resolve 되면 사용자의 intent가 계속 진행될 수 있는지 확인할 때 입니다. 제공된 Intent를 완료하기 위한 모든 요구 사항을 앱이 가지고 있는지 보고합니다. 이 단계에서는 Siri에게 예상되는 결과 및 필요한 경우, 확인 요청을 알립니다. resolution method와 유사하나, intent 당 하나만 존재합니다.

```Swift
func confirm(requestRide intent: INRequestRideIntent, completion: @escaping (INRequestRideIntentResponse) -> Void) {
  let responseCode: INRequestRideIntentResponseCode
  if let location = intent.pickupLocation?.location,
    simulator.pickupWithinRange(location) {
    responseCode = .ready
  } else {
    responseCode = .failureRequiringAppLaunchNoServiceInArea
  }
  let response = INRequestRideIntentResponse(code: responseCode, userActivity: nil)
  completion(response)
}
```


#### 2.3 Handle

최종적이고 가장 중요한 단계입니다. 여기서 **앱이 Intent에 대한 실제 처리를 수행**하고 작업이 성공적으로 완료된 것을 보고합니다. 그러나 경우에 따라 오류가 발생할 수 있습니다. 또한 느린 네트워크 연결과 같은 요청을 완료하는 데 더 많은 시간이 필요한 경우 사용자가 Siri 상호 작용을 종료한 후에도 작업이 계속된다는 것을 Siri에게 알릴 수 있습니다. 아래의 예시에서는 INRequestRideIntentResponse가 모든 정보를 캡슐화하여 객체로 가지고 있을 것 입니다.

```Swift
func handle(intent: INRequestRideIntent,
    completion: @escaping (INRequestRideIntentResponse) -> Void) {

      // 1
  guard let pickup = intent.pickupLocation?.location else {
    let response = INRequestRideIntentResponse(code: .failure,
      userActivity: .none)
    completion(response)
    return
  }

  // 2
  let dropoff = intent.dropOffLocation?.location ??
    pickup.randomPointWithin(radius: 10_000)

  // 3
  let response: INRequestRideIntentResponse
  // 4
  if let balloon = simulator.requestRide(pickup: pickup, dropoff: dropoff) {
    // 5
    let status = INRideStatus()
    status.rideIdentifier = balloon.driver.name
    status.phase = .confirmed
    status.vehicle = balloon.rideIntentVehicle
    status.driver = balloon.driver.rideIntentDriver
    status.estimatedPickupDate = balloon.etaAtNextDestination
    status.pickupLocation = intent.pickupLocation
    status.dropOffLocation = intent.dropOffLocation

    response = INRequestRideIntentResponse(code: .success, userActivity: .none)
    response.rideStatus = status
  } else {
    response = INRequestRideIntentResponse(code: .failureRequiringAppLaunchNoServiceInArea, userActivity: .none)
  }

  completion(response)

}
```

사용자가 shortcut을 누를 수 있고 Siri가 이 방법으로 앱을 열 것이기 때문에 AppDelegate에서는 항상 다음 항목을 실행합니다.

```Swift
func application(_ application: UIApplication,
         continue userActivity: NSUserActivity,
         restorationHandler: @escaping ([UIUserActivityRestoring]?) -> Void) -> Bool
```


### Phase 3: Handling Intent UI (Optional)

 INUIHostedViewControlling 프로토콜에 따라 intent ui를 커스터마이징 할 수 있습니다. 각 Intents UI 확장에는 하나의 뷰 컨트롤러만 포함되어야 하지만, 해당 뷰 컨트롤러는 여러 intent를 지원할 수 있습니다.

> 기본 Siri 또는 Maps 인터페이스를 유지하고 사용자 지정 콘텐츠로만 확장하려면 **configure(with: context: completion:) method** 안에서 뷰 컨트롤러를 구성할 수 있습니다.

> Default Interface의 일부 혹은 전부를 교체하고 싶다면, **configureView(for:of:interactiveBehavior:context:completion:) method** 안에서 뷰 컨트롤러의 루트 뷰를 구성할 수 있습니다.


<img src="https://docs-assets.developer.apple.com/published/b0271f8ff5/5f270d25-5eff-472c-84a7-7c2e4612c0cf.png"  width='500'>


### Phase 4: Shortcuts

이전에는 SiriKit SDK의 전반적인 기능이 상당히 제한적이었습니다. Siri Shortcuts으로 개발자들은 Sirikit의 기능을 확장하고 앱을 호출하는 사용자 지정 음성 동작을 만들 수 있습니다. 즉, 모든 앱의 빠른 행동(Quick Action)을 미리 등록해서 사용할 수 있게 됩니다. 일종의 ‘바로가기’인 ‘숏컷’은 **사용자 맞춤식 음성 명령**으로, 시리에게 하나의 단어만으로 **특정 명령**을 수행토록 하는 간편한 기능입니다.

 사용자가 수행하지 않은 작업이지만 흥미를 유발하는 shortcut에 대해 앱이 해당 shortcut을 **1) suggestion** 하거나, 앱이 siri에게 사용자가 완료한 작업에 대한 제안을 하는 데에 필요한 정보를 **2) donate** 하여 siri로 하여금 특정 shortcut을 알게 함으로써 shortcut을 등록 후 관리합니다.


 > INUIAddVoiceShortcutViewController에서 Siri에 shortcut 추가를 관리하며, INUIEditVoiceShortcutViewController에서 존재하는 shortcut의 수정 및 삭제를 담당합니다.

#### 4.1 Suggesting Shortcuts to Users

사용자가 수행하지 않았지만 Siri에 추가하려는 작업에 대한 바로 가기를 제안하려면 작업을 정의하는 INIntent 또는 NSUserActivity 개체를 사용하여 INShortcut 개체를 만듭니다. 그런 다음 리스트에 shortcut을 추가합니다. 앱에 제안하고자 하는 suggestion마다 반복합니다. list of shortcut suggestions를 만든 후 setShortcutSugestions(_:)를 콜 하여 shortcut을 전달합니다.



```Swift
import Intents

// Add a user activity to the list of suggestions.
var suggestions = [INShortcut(userActivity: orderFavoriteBeverageUserActivity)]

// Add an intent to the list of suggestions. To create
// a shortcut from an intent, the intent must be valid.
if let shortcut = INShortcut(intent: orderSoupOfTheDayIntent) {
    suggestions.append(shortcut)
}

// Suggest the shortcuts.
INVoiceShortcutCenter.shared.setShortcutSuggestions(suggestions)
```

#### 4.2 Donating Shortcuts

**"Donating"이란 사용자가 방금 완료한 작업에 대한 제안을 하는 데 필요한 정보(User context)를 Siri에게 제공하는 것**을 의미합니다. Siri는 당신의 앱이 Siri에게 주는 Donate를 통해 당신의 앱이 이용할 수 있는 Shortcut를 배웁니다. 시리는 사용자의 기부된 의도에서 패턴을 찾고 관련 제안을 정기적으로 시도합니다. Siri는 사용자 지정 목적에 대해 서로 다른 매개변수 조합을 정의함으로써 donate intent의 추세와 그에 상응하는 조합을 검토하여 좋은 제안을 할 수 있습니다.

사용자가 앱에서 삭제한 관련 없는 정보가 포함된 donate된 intent를 삭제하는 것이 중요합니다. 또한 사용자가 Siri Intent Extension에서 처리되는 동안 사용자가 수행하지 않은 작업을 포함하지 않습니다. 예를 들어 사용자가 앱으로 식당에서 스프를 주문하는 경우, 사용자가 주문을 마친 후 스프 주문 작업의 Shortcut에 Donate 하지만 주문을 완료하지 않은 경우 Donate하지 않습니다.

NSUserActivity 개체 또는 INInteraction 개체를 사용하여 Donate 할 수 있습니다.

- **1) Donate a User Activity**

>NSUserActivity는 Handoff 및 Spotlight 검색과 같은 다른 Apple 기능과 통합되는 Donation을 위한 가벼운 방식을 제공합니다. 어플리케이션이 유저 context를 이해하도록 도와줍니다.

>NSUserActivity를 사용하여 Donate하려면 Info.plist의 **NSUserActivityTypes** 배열에 활동을 정의합니다. 활동 타입은 목록 내에서 고유한 reverse domain name 이어야 합니다.
앱에서 NSUserActivity의 인스턴스를 만들고 나중에 작업을 다시 시작하는 데 필요한 정보를 사용하여 title, userInfo 및 requiredUserInfoKeys를 설정합니다.

>또한 isEligibleForPreditation 속성을 true로 설정하고 perstantIdentifier를 Donation을 삭제하는 데 필요한 Unqiue 문자열 값으로 설정합니다.


- **2) Donate INInteraction**

>  다른 방법은 **INInteraction** 개체를 사용하는 것입니다. 여기에는 조금 더 많은 작업이 포함되지만 작업을 정의하고 처리할 수 있는 제어력이 향상됩니다.

>소스 코드를 보기 전 시스템 제공 항목의 목록을 검토하여 작업에 적합한 도메인이 있는지 확인합니다. 작업을 가장 잘 설명하는 Intent를 사용합니다. 그렇지 않으면 Custom Intent를 생성합니다.

```Swift
func makeTransfer(from sender: String, to receiver: String, amount: Decimal) {
        let intent = TransferMoneyIntent()
        intent.amount = INCurrencyAmount(amount: NSDecimalNumber(decimal: amount), currencyCode: "USD")
        intent.fromAccount = sender
        intent.toAccount = receiver
        let interaction = INInteraction(intent: intent, response: nil)
        interaction.donate { error in
            guard error == nil else {
                print("Could not donate the intent")
                return
            }
            print("Successcully donated the intent!")
        }
    }
```

#### 4.3 Relevant Shortcuts

사용자가 수행한 intent를 donate하는 것 외에도, 앱은 이제 Siri가 향후 적절한 시기에 추천할 수 있도록 사용자 지정 가능한 Shortcut을 공유할 수 있습니다. 즉, 앱에서 이 작업을 권장하는 시기 또는 위치에 대한 정보를 첨부할 수 있습니다.

> 1) INShortcut instance로 INRelevantShortcut 객체를 생성합니다.

> 2) 그런 다음 relivanceProviders 속성을 사용하여 INRefilityProvider 값의 배열을 할당합니다.

> 3) 그런 다음 INRehibleShortcutStore를 가져오고 setRepectsShortcuts를 호출하여 Siri가 앱이 수행할 수 있는 작업을 인식하도록 합니다.

매일 저녁 transfer를 권유하는 예시 시나리오 입니다.

```Swift
func makeTransfer(from sender: String, to receiver: String, amount: Decimal) {
      let intent = TransferMoneyIntent()
      intent.amount = INCurrencyAmount(amount: NSDecimalNumber(decimal: amount), currencyCode: "USD")
      intent.fromAccount = sender
      intent.toAccount = receiver
      guard let shortcut = INShortcut(intent: intent) else {
          return
      }
      let relevantShortcut = INRelevantShortcut(shortcut: shortcut)
      relevantShortcut.relevanceProviders = [INDailyRoutineRelevanceProvider(situation: .evening)]
      INRelevantShortcutStore.default.setRelevantShortcuts([relevantShortcut], completionHandler: nil)
  }
```
