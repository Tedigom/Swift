# Memory Safety

기본적으로 Swift는 코드가 비정상 적으로 동작하는 것을 막는 행위를 합니다. 예를 들면, 변수가 초기화 되기 전에 사용된다던가, 메모리에서 해제된 값을 접근하는 것을 막는 다던가, 인덱스의 한계를 넘는지 확인한다는 것 들이 그것 입니다. Swift에서는 또 메모리의 같은 영역을 동시에 접근해서 충돌이 나지 않도록 해줍니다. 이렇듯 Swift에서 메모리 관련해 자동으로 관리해 주기때문에 대부분의 경우에는 Swift언어를 사용하는 사용자는 메모리의 접근에 대해 전혀 생각하지 않고 사용해도 됩니다. 하지만 메모리 접근 충돌이 발생할 수 있는 잠재적인 상황을 이애하고 메모리 접근 충돌을 피하는 코드를 어떻게 작성할 수 있는지 이해하는 것은 중요합니다. 만약 메모리 접근 충돌이 일어나면 런타임 에러나, 컴파일 에러가 발생합니다.


## 메모리 접근 충돌의 이해

코드에서 메모리 접근은 아래 예와 같이 변수에 값을 할당하거나 접근할 때 발생합니다.

```swift
// A write access to the memory where one is stored.
var one = 1
​
// A read access from the memory where one is stored.
print("We're number \(one)!")
```
위 코드는 one이라는 변수에 1을 할당하고 print하기 위해 one을 접근해 사용합니다. **메모리 충돌은 메모리에 값을 할당하고 메모리 값을 접근하는 것을 동시에 수행할때 발생** 합니다. 만약 동시성(cuncurrent) 코드나, 멀티쓰레드 코드를 작성한적이 있다면 이 메모리 접근 충돌 문제는 익숙한 문제일 것 입니다. 하지만 이 **접근 충돌 문제는 싱글 쓰레드에서 발생할 수 있는 문제** 이고 동시성과 멀티쓰레드와 관련이 없습니다. 예를 들어보겠습니다. 만약 아래와 같이 특정 물건을 구매하고 구매 총 금액을 확인하는 경우 Before의 기본 상태에서 Total은 5 달러 입니다. 만약 TV와 T-shirt 를 구매하는 동안(During) Total에 접근해 값을 가져 왔다면 Total은 5 달러가 됩니다. 하지만 실제 제대로 된 값은 320 달러여야 할 것 입니다.

<img src="https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LKLx6PA5iF3Uq2IzQsf%2F-LKMdbZHEeIzNLG5Op8j%2F-LKMdecqB_rebeymKadF%2Fmemory_shopping_2x.png?alt=media&token=451f68a5-24d9-4f5a-a684-8b836b350ac7" width = 600>


메모리 접근이 충돌 할 수 있는 상황의 성격은 3가지 경우가 있습니다. 메모리를 읽거나 쓰는 경우, 접근 지속시간 그리고 메모리가 접근되는 위치입니다. 구체적으로 메모리 충돌은 다음 3가지 조건 중 2가지를 만족하면 발생합니다.

- 최소 하나의 쓰기 접근 상황

- 메모리의 같은 위치를 접근할 때

- 접근 지속시간이 겹칠 때

대표적으로 세 가지 상황에서의 메모리 충돌을 살펴보고자 합니다.

### 1. Conflicting Access to In-Out Parameters

메모리 충돌은 in-out 파라미터를 잘못 사용할 때 발생할 수 있습니다. 아래 예제 코드를 보시겠습니다.

```swift
var stepSize = 1
​
func increment(_ number: inout Int) {
    number += stepSize
}
​
increment(&stepSize)
// Error: conflicting accesses to stepSize
```

increment의 파라미터로 inout Int의 number를 사용합니다. 그리고 함수 내부에서 인자로 사용한 number를 변경합니다. 이 경우 인자로 number를 넣고, 또 number를 읽어 그 number에 stepSize를 추가해 다시 할당하는 쓰기와 읽기가 아래 그림과 같이 동시에 발생해 접근 충돌이 일어 납니다.

<img src="https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LKLx6PA5iF3Uq2IzQsf%2F-LKMdbZHEeIzNLG5Op8j%2F-LKMe3tD5hbF8K7c8Ais%2Fmemory_increment_2x.png?alt=media&token=b5a2a3ef-0835-4a62-8b77-e99a1c58393b" width = 600>

이 문제를 해결하기 위한 한가지 방법은 stepSize의 복사본을 명시적으로 사용하는 것입니다. 아래 코드와 같이 stepSize를 복사한 copyOfStepSize를 사용하면 하나의 메모리를 읽고 쓰는 행위를 동시에 하지 않게돼 접근 충돌을 피할 수 있습니다.

```swift
// Make an explicit copy.
var copyOfStepSize = stepSize
increment(&copyOfStepSize)
​
// Update the original.
stepSize = copyOfStepSize
// stepSize is now 2
```

또 다른 in-out 파라미터의 장기 접근(long-term)으로 인한 충돌을 다음과 같은 상황에서 일어날 수 있습니다.

```swift
func balance(_ x: inout Int, _ y: inout Int) {
    let sum = x + y
    x = sum / 2
    y = sum - x
}
var playerOneScore = 42
var playerTwoScore = 30
balance(&playerOneScore, &playerTwoScore)  // OK
balance(&playerOneScore, &playerOneScore)
// Error: conflicting accesses to playerOneScore
```

이 경우 balance(&playerOneScore, &playerOneScore)와 같이 인자를 넣으면 읽기와 쓰기를 동시에 하게 돼서 접근 충돌이 발생합니다.

### 2. Conflicting Access to self in Methods


매소드 안에서 self에 접근할때 충돌이 발생할 수도 있습니다. 예를 들기 위해 아래와 같이 Player구조체를 선언합니다.

```swift
struct Player {
    var name: String
    var health: Int
    var energy: Int
​
    static let maxHealth = 10
    mutating func restoreHealth() {
        health = Player.maxHealth
    }
}
```

이 구조체를 확장해 Player간에 체력을 공유하는 shareHealth함수를 익스텐션에서 선언합니다. Player타입의 teammate는 inout파라미터로 지정하고 동작은 현재 Player와 입력한 teammate Player간에 balance함수를 실행합니다. 여기서 사용하는 인자는 in-out 파라미터입니다.

```swift
extension Player {
    mutating func shareHealth(with teammate: inout Player) {
        balance(&teammate.health, &health)
    }
}
​
var oscar = Player(name: "Oscar", health: 10, energy: 10)
var maria = Player(name: "Maria", health: 5, energy: 10)
oscar.shareHealth(with: &maria)  // OK
```

이 경우는 oscar와 maria둘다 다른 구조체 인스턴스 이기 때문에 체력을 공유해도 아래와 같이 아무 문제가 없습니다.

<img src = "https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LKLx6PA5iF3Uq2IzQsf%2F-LKMdbZHEeIzNLG5Op8j%2F-LKMeKXBJ29HOtoVQwWC%2Fmemory_share_health_maria_2x.png?alt=media&token=afbbdfad-3605-4a75-a85d-a5d0120f7cb7" width = 600>

하지만 만약 oscar를 자신과 같은 인스턴스인 oscar와 체력을 공유한다고 실행하면 체력을 읽어오고 읽어온 체력을 변경하는 동작을 한 메모리 위치에서 동시에 수행하게 돼서 충돌이 발생합니다.

```swift
oscar.shareHealth(with: &oscar)
// Error: conflicting accesses to oscar
```

<img src = "https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LKLx6PA5iF3Uq2IzQsf%2F-LKMdbZHEeIzNLG5Op8j%2F-LKMeP8tSWTSTp_Ls6gn%2Fmemory_share_health_oscar_2x.png?alt=media&token=61c995ff-494a-47f5-9657-90ccd989f70c" width = 600>

### 3. Conflicting Access to Properties

프로퍼티 접근시에도 충돌이 발생할 수 있습니다. 아래와 같이 전역변수를 가지고 읽기 쓰기를 동시에 수행하는 balance func를 실행할때 충돌 에러가 발생합니다.

```swift
func balance(_ x: inout Int, _ y: inout Int) {
    let sum = x + y
    x = sum / 2
    y = sum - x
}

var playerInformation = (health: 10, energy: 20)
balance(&playerInformation.health, &playerInformation.energy)
// Error: conflicting access to properties of playerInformation

var holly = Player(name: "Holly", health: 10, energy: 10)
balance(&holly.health, &holly.energy)  // Error
```

그러나 아래와 같이 지역변수에서 balance를 수행하는 경우 충돌 에러가 발생하지 않습니다.

```swift
func someFunction() {
    var oscar = Player(name: "Oscar", health: 10, energy: 10)
    balance(&oscar.health, &oscar.energy)  // OK
}
```

구조체에서 프로퍼티를 접근하는데 오버레핑 접근으로부터 안전한 상황은 다음과 같습니다.

- 구조체 인스턴스에서 저장프로퍼티에만 접근하고 계산된 프로퍼티 혹은 클래스 프로퍼티를 접근하지 않을 때

- 구조체가 전역변수가 아니라 지역변수 일때

- 구조체가 어떤 클로저도 획득(capturing)하지 않거나 획득한 캡처가 nonescaping 클로저 일때

만약 컴파일러가 접근이 안전하다고 판단하지 못하면 접근이 불가능 합니다.

### Reference:

- https://jusung.gitbook.io/the-swift-language-guide/untitled-20
