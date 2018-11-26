# Error Handling

프로그램 실행시 에러가 발생하면 그 상황에 대해 적절한 처리가 필요합니다. 이 과정을 에러 처리라고 부릅니다. Swift에서는 런타임에 에러가 발생한 경우 그것의 처리를 위해 에러의 발생(throwing), 감지(catching), 증식(propagating), 조작(manipulating)을 지원하는 일급 클래스(first-class)를 제공합니다.

어떤 명령은 항상 완전히 실행되는 것이 보장되지 않는 경우가 있습니다. 그런 경우에 옵셔널을 사용해 에러가 발생해 값이 없다는 것을 표시할 수 있지만, 어떤 종류의 에러가 발생했는지 확인할 수는 없습니다. 이럴 때는 구제적으로 발생한 에러를 확인할 수 있어야 코드를 작성하는 사람이 각 에러의 경우에 따른 적절한 처리를 할 수 있습니다.

예를들어, 디스크에서 파일을 읽어 데이터를 처리하는 일을 한다고 할 때 이 작업이 실패할 경우의 수는 여러가지가 존재합니다. 파일이 특정 경로에 존재하지 않거나, 읽기 권한이 없거나 혹은 파일의 데이터가 식별할 수 있는 포맷으로 적절히 인코딩 되지 않은 경우 등 종류별 에러 상황을 식별해 사용자에게 제공해 주면 프로그램 실행 중 발생한 각 에러별로 사용자가 적절히 대응할 수 있도록 도울 수 있습니다.

## Representing and Throwing Errors

Swift에서 에러는 Error 프로토콜을 따르는 타입의 값으로 표현됩니다.

Swift의 열거형은 특별히 이런 관련된 에러를 그룹화(Grouping)하고 추가적인 정보를 제공하기에 적합합니다. 예를들어, 게임 안에서 판매기기 동작의에러 상황을 다음과 같이 표현할 수 있습니다.

```swift
enum VendingMachineError: Error {
     case invalidSelection
     case insufficientFunds(coinsNeeded: Int)
     case outOfStock
}
```

에러를 발생시킴으로써 무언가 기대하지 않았던 동작이 발생했고 작업을 계속 수행할 수 없다는 것을 알려줄 수 있습니다. 어떤 함수, 매소드 혹은 초기자가 에러를 발생 시킬 수 있다는 것을 알리기 위해서 throw 키워드를 함수 선언부의 파라미터 뒤에 붙일 수 있습니다. throw 키워드로 표시된 함수를 throwing function이라고 부릅니다. 만약 함수가 리턴 값을 명시했다면 throw 키워드는 리턴 값 표시 기호인 -> 전에 적습니다.

```swift
func canThrowErrors() throws -> String
​
func cannotThrowErrors() -> String
```

## Handling Errors

에러가 발생하면 특정 코드영역이 해당 에러를 처리하도록 해야합니다. 예를들어, 문제를 해결하거나, 혹은 우회할 수 있는 방법을 시도하거나 아니면 사용자에게 실패 상황을 알리는 것이 에러 처리의 방법이 될 수 있습니다.

Swift에서 4가지 에러를 처리 방법이 있습니다.

1. 에러가 발생한 함수에서 리턴값으로 에러를 반환해 해당 함수를 호출한 코드에서 에러를 처리하도록 하는 방법 (guard 등)

2. 두번째로 do-catch 구문을 사용하는 방법

3. 셋째로 옵셔널 값을 반환하는 방법 (try? 사용)

4. 마지막으로 강제로 에러 발생을 중지하기 (try! 사용)

### Reference:

- https://jusung.gitbook.io/the-swift-language-guide/untitled-13
