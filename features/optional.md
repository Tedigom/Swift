# Optional

## nil

어떠한 value도 없는 경우를 말합니다.다만 유의할 점은 Swift의 nil 은 다른 언어에서 pointer가 존재하지 않는 값을 가리키는 것과는 다릅니다. (Reference or value type)

## Optional 이란?

Swift에는 optional이라는 개념이 있습니다. optional은 ‘?’을 통해 표현되는데, 그 의미는 다음과 같습니다. “이 변수에는 값이 들어갈 수도 있고, 아닐 수도 있어(nil)” Swift에서는 기본적으로 변수 선언시 nil 값이 들어가는 것을 허용하지 않습니다. 그러므로 첫 번째 줄의 코드는 에러이고, 두 번째 줄은 Optional type(String?)으로 선언했으므로 에러가 아닙니다. Optional은 nil값으로 인해 일어나는 프로그램의 잠재적 오류를 보완하기 위해 탄생된 일종의 안전장치 입니다.

```swift
var optionalString: String = nil
var optionalString: String? = nil
```

## Wrapping

<img src="./assets/wrapping.png">

Optional에 대해 보다보면, 많은 곳에서 wrapping 이라는 개념이 나옵니다. Optional 타입은 기본적으로 wrap되어 있는 상태입니다. 즉, Optional 변수들은 제대로된 value가 있는 것인지, nil인 것인지 wrap되어 있어서 모르는 상태라고 생각하시면 됩니다. 그렇기 때문에(컴파일러 입장에서는 변수가 nil일 수도 있기 때문에) wrap된 상태에서는 설령 변수에 value값이 있다고 하더라도 바로 value가 출력되지 않습니다.

```swift
var optionalString: String? = "Hello"
print(optionalString)
// 출력 결과: Optional("Hello")
```
이 경우, optionalString이 nil일 수도 있기 때문에, 결과값 “Hello”가 출력되지 않고, Optional(“Hello”) 가 출력됩니다.

## Optional의 장점

프로그램 사용 도중 nil값이 될 수 있는 변수들로 인해 일어나는 **잠재적 오류를 예방**할 수 있습니다.

## Optional 접근 방법

### Force Unwrapping

Optional Unwrapping이란 Optional 변수에서 Optional 껍데기를 벗겨내는 작업입니다. Optional로 선언했지만, 무조건 변수가 있는 상황이 보장된 경우 느낌표(!)를 쓰면 강제적으로 Optinal변수를 Unwrapping합니다. 다만, Forced unwrapping은 쉽게 에러를 낼 수 있기 때문에 항상 값이 있다고 보장할 수 있는 경우에만 사용하는 것이 권장됩니다.

```swift
let value2: String! = nil
if value2 != nil {
    print(value2)
}
```

### Optional Binding

Optional Binding은 Optional 타입으로 선언된 변수에 값이 있는지 없는지를 확인할 수 있도록 해주는 기능입니다. Optional Binding을 사용하면 느낌표 없이 Optional 타입의 변수 값을 출력할 수 있어서 좀 더 안전한 형태로 값을 얻을 수 있습니다. 기본적인 형태는 다음과 같습니다.

```swift
if let 변수명 = Optional 변수 {
  // 임시 변수에 Optional 변수의 value값이 할당됩니다.
}
```
아래는 예시입니다.

```swift
// Optional type으로 선언한 myNumber
let myNumber: Int? = 1234

if let actualNumber = myNumber {
    print("\(myNumber)은 실제로 \(actualNumber)입니다.")
} else {
    print("\(myNumber)는 변환될 수 없습니다.")
}
// 출력 결과 : Optional(1234)은 실제로 1234입니다.

print(actualNumber) // error
```
위의 예에서는 myNumber가 Optional 타입으로 선언되어 있습니다. 원래는 이 myNumber값을 출력하기 위해서는 !를 사용해야합니다. 하지만, Optional Binding은 먼저 이 myNumber의 값이 있는 경우와 없는 경우로 나누고, 값이 있는 경우를 if let 구문 안에 넣을 수 있습니다. 여기서는 actualNumber에 myNumber의 값을 할당하고, 값이 있다면 actualNumber에 이를 넘겨주어 바로 실제 값으로 사용할 수 있도록 해줍니다. 특이한 점은 actualNumber가 if문 안에서만 할당되는 로컬 변수라는 점입니다. if 밖에서는 actualNumber를 사용할 수 없습니다.

> 즉, Optional Binding은 Optional type의 변수에 대한 nil 체크와 로컬변수에 이 값을 할당하는 두 가지 기능을 가지고 있습니다.

### Optional Chaining

여러 객체를 혼합해서 사용하다보면 Optional끼리의 연산이 필요한 경우가 있습니다. 이 경우에 객체마다 옵셔널 바인딩을 사용하게 되면, if문이 지나치게 중첩되는 결과가 발생할 것 입니다. 이럴 때에 Optional Chaining을 통해서 좀 더 간단하게 Optional 예외처리를 할 수 있습니다. 여러 개의 query가 묶여서 나타날 수 있고, 전체 연결은 하나라도 nil이라면 nil을 반환합니다.

```swift
if let roomCount = john.residence?.numberOfRooms {
    print("residence에 값이 있습니다. 값 : \(roomCount)")
} else {
    print("residence is nil")
}
# residence is nil 출력
```
이 때, if let roomCount = john.residence?.numberOfRooms은 해당 chain에(residence?와 numberOfRooms) 값이 있는지 없는지를 체크하고, 하나라도 없으면 false를 반환합니다.


### Implicitly Unwrapped Optional

변수 선언 시에 무조건 값이 존재할 것이라고 선언하여 주는 것입니다. 이 역시 강제 언래핑과 함께 주의해서 사용해야 합니다. 객체가 생성된 이후에 nil이 될 수 없는게 확실한 경우 사용가능합니다.

```swift
@IBOutlet weak var collectionView: UICollectionView!
```

### With

옵셔널만을 위해 존재하는 것은 아니지만 옵셔널과 함께 유용하게 쓸 수 있는 몇 가지의 옵션이 존재합니다.

### 1. guard

```swift
guard let strongSelf = self else { return }
```

### 2. as

타입 캐스팅을 시도합니다.

> as?: 해당 타입에 대해 맞으면 옵셔널 바인딩 아니면 nil로 처리합니다.

> as!: 강제적 타입 변환을 시도합니다. 변환이 되지 않으면 크래쉬가 발생하기 때문에 가급적 사용하지 않는 것이 좋습니다.

### 3. try

> try?: 시도하여 언래핑이 되지 않으면 nil이니 다른 조치를 취합니다.

> try!: 강제적으로 언래핑을 시도 합니다.

### Reference:
- https://hcn1519.github.io/articles/2017-01/swift_optionals
