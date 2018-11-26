# Single Responsibility Principle (단일 책임 원칙)

"단일 클래스는 오직 하나의 책임을 가져야 한다. (클래스를 수정/변경하는 이유는 단 한개 여야 한다)"는 원칙입니다.

단일 책임 원칙은 클래스의 목적을 명확히 함으로써 구조가 난잡해지거나 수정 사항이 불필요하게 넓게 퍼지는 것을 예방하고 기능을 명확히 분리할 수 있게 합니다. 하나의 클래스가 두 가지 이상의 책임을 지니게 되면 클래스의 목적이 모호해지고 기능을 수정할 때 영향을 받는 범위도 커져서 유지보수가 힘들어지며 결국 작성한 본인조차도 이게 정확히 뭐하는 클래스인지 명확히 설명할 수가 없는 스파게티 코드가 되어버리곤 합니다.

이를 명확히 하기 위하여 '단일'과 '책임' 두 가지 키워드에 대한 이해가 필요합니다.

## 단일

“단일”이란 결국 “한가지”를 말한다고 이해할 수 있습니다. 다만, “단일”의 범위가 문제가 될 수 있습니다. 정말 크게 봐버리면 앱 자체가 이미 한가지 기능을 가지고 있다고 이야기 해버릴 수도 있습니다. 정말 작게 본다면 앱의 특정한 부분의 아주 세부적인 특정 기능을 이야기 할 수 도 있을 것입니다.

이렇듯 “단일”이라는 표현은 매우 상대적인 부분입니다. 때문에 아래의 "책임"부분과 함께 고려하여 적당한 선을 개발자 스스로 선택해야 합니다.

## 책임

SRP에서는 “책임”을 **변경을 위한 이유** 라고 판단합니다. 만일 특정 한가지의 기능을 변경하기 위해서 여러 클래스를 고쳐야 한다면 기본적으로 설계가 잘못 되었을 가능성이 큽니다. 동일한 수정은 한 곳에서 이루어져야 한다는 것입니다.

반대로, 특정 기능을 변경하기 위해 수정을 했으나 클래스의 대부분의 코드가 수정되지 않았다면 (클래스의 극히 일부만 수정되었다면) 이 역시 SRP 위반 사례가 될 수 있습니다. 즉, 그 클래스는 그 기능 이외의 여러가지 기능들을 가지고 있는 상태라고 인식할 수 있습니다.

만일 하나의 클래스가 있는데 논리적으로 생각했을 때는 단일 기능이라 판단되지만 차후 변경의 관점에서 특정 부분만 변경될 가능성이 높거나 빈번하다면 그 변경부만 별도의 클래스로 분리하는 것이 SRP를 준수하는 방법이 됩니다. 제대로 된 설계에서는 클래스는 응집력을 가지고 있고 결합도가 낮으며 특정 기능의 변경을 위한 수정이 한곳에 집중 되어야 합니다. 그런 경우 SRP를 준수했다고 할 수 있습니다.

## SRP 위배

SRP를 위배한다는 것은 클래스가 이러한 단일 책임을 지지 않는다는 것을 말합니다.

아래는 하나의 UserSettingService class가 1. 변경과 2. 접근 권한의 두 가지 책임을 지는, SRP 위배의 예시입니다.

```swift
struct UserSettingService {

    func changeEmail(_ user: User) {
        if(checkAccess(user)) {
        // Grant option to change
        }
    }

    func checkAccess(_ user: User) -> Bool {
        // Verify if the user is valid.
    }

}
```

위의 예시에서 SecurityService class를 추가하여 두 개의 책임을 나눠갖도록 리팩토링하면 아래와 같습니다. 

```swift
struct UserSettingService {
    func changeEmail(User user) {
        if(SecurityService.checkAccess(user)) {
        // Grant option to change
        }
    }
}

struct SecurityService {
    static func checkAccess(User user) -> Bool {
        // Verify if the user is valid.
    }
}
```

메서드의 SRP를 해치는 경우 자주 나타나는 대표적인 코드가 반복적인 분기문입니다. (수정에 대해 닫혀있지 않다는 점에서 OCP의 위배이기도 합니다.) 아래는 SRP 원칙을 지키지 못한 경우입니다.

```swift
class Person {
    private static let MEN: Int = 1
    private static let WOMEN: Int = 2
    private var gender: Int = 1

    func wearUnderwear() {
        if (self.gender == Person.MEN) {
        // 아래 속옷만 입는다
        } else {
        // 아래,위로 속옷을 입는다
        }
    }
}
```
wearUnderwear메서드가 Men과 Women의 행위를 모두 구현하려고 하기 때문에 단일 책임의 원칙을 위배한다고 볼 수 있습니다. 이를 리팩토링하면 다음과 같습니다.

```swift
protocol Person {
    func wearUnderwear()
}

class Men: Person {
    func wearUnderwear() {
        // 아래 속옷만 입는다
    }
}

class Women: Person {
    func wearUnderwear() {
        // 위 아래로 속옷을 입는다.
    }
}
```



### iOS에서의 대표적인 SRP 위반 사례

iOS에서 가장 대표적이고 공통적으로 존재하며 많이 언급되는 SRP위반사례는 가장 많이 사용하는 UIViewController의 subclass들입니다. 통상 massive view controller라는 용어로 비대해진 view controller에 대한 성토가 있고 그 해결책으로 MVP, MVVM, VIPER등의 패턴들을 이야기 하고 있습니다.

다음은 애플 문서에서 정의한 UIViewController입니다.

Provides the infrastructure for managing the views of your UIKit app.

“managing views”라고 정확하게 정의를 내리고 있습니다. 통상 ViewController의 코드는 꽤 길 것으로 생각됩니다. 해당 클래스에서 “managing views”에 해당하지 않는 부분인 데이터 패치 및 관리, 엄격하게는 View의 layout에 밀접하게 관여하는 것까지 SRP 위반이라고 볼 수 있을 것 입니다.

### Reference:

- https://namu.wiki/w/%EA%B0%9D%EC%B2%B4%20%EC%A7%80%ED%96%A5%20%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%B0%8D/%EC%9B%90%EC%B9%99

- https://jungwoon.github.io/solid/2017/07/31/Solid-Principle/

- http://karenn.tistory.com/11
