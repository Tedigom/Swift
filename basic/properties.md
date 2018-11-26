# Properties

클래스, 구조체, 열거형 안에 선언되어서 사용하는 '소속된 변수' 입니다.

## 1.Instance Property

### 1.1 Stored instance property

Stored instance Property(저장 프로퍼티)는 상수(constant)와 변수(variable)값을 인스턴스의 일부로 메모리에 저장합니다. 클래스와 구조체에서만 사용됩니다

```swift
class FixedLengthRange {
    var firstValue: Int
    let length: Int

    init(firstValue : Int, length:Int) {
        self.firstValue = firstValue
        self.length = length
    }
}
```

위의 예시에서 firstValue와 length를 Stored property라고 말할 수 있습니다.

#### Lazy Stored Properties

값이 사용되기 전까지는 값이 계산되지 않는 프로퍼티입니다. 초기값이 인스턴스의 초기화가 될때까지 값을 모르는 외부요소에 의존하는 경우나 초기값이 복잡하거나 계산비용이 많이 드는 설정을 필요로 할때 유용합니다.

이러한 lazy 프로퍼티는 항상 변수로서 선언해야 합니다. 또한 lazy로 선언했다고 해도 lazy 프로퍼티가 초기화 되지 않은 상태에서 여러 쓰레드가 동시에 lazy 프로퍼티에 액세스 한다면, 이 프로퍼티가 단 한번만 초기화 된다는 것을 보장할 수 없습니다.


### 1.2 Computed instance property

(인스턴스의 상태를 바탕으로) 값을 연산하여 반환합니다. 클래스, 구조체 그리고 열거형에서 사용됩니다. 클래스, 구조체, 열거형은 저장 프로퍼티 이외에도 값을 저장하지 않는 연산 프로퍼티를 정의할 수 있는데, getter와 setter를 통해 다른 프로퍼티와 간접적으로 값을 검색하고 세팅합니다. get, set의 기본 syntax는 아래와 같습니다.

```swift
var myProperty: Int {
   get {
      return myProperty
   }
   set(newVal) {
      myProperty = newVal
   }
}
myProperty = 123 // warning
```

get과 set은 해당 프로퍼티에 직접 붙어있기 때문에 위와 같이 get{}, set{}에서 myProperty에 접근하면 recursive하게 자기 자신의 get, set을 호출하게 되므로 이렇게 사용하면 안됩니다. 일반적으로 get, set은 아래처럼 실제 값을 저장 할 backing storage가 필요합니다.

```swift
var _myProperty: Int
var myProperty: Int {
   get {
      return _myProperty
   }
   set(newVal) {
      _myProperty = newVal
   }
}
```

_myProperty는 실제로 값이 저장되는 변수입니다. 외부에서 myProperty의 값에 접근하거나 새로운 값을 할당하면 실제로 값이 저장되는 곳은 myProperty가 아닌 _myProperty입니다. 즉 myProperty는 _myProperty의 interface역할을 합니다.

이러한 get, set을 사용하는 경우는 다음과 같습니다.

1. 프로퍼티에 값이 할당 될 때 적절한 값인지 검증하기 위해
  ex) 음수 검증


2. 다른 프로퍼티값에 의존적인 프로퍼티를 관리 할 때

3. 프로퍼티를 private하게 사용하기 위해

## 2. Type property

인스턴스 property는 특정 타입의 인스턴스에 속한 property입니다. 해당 타입의 인스턴스가 새로 생성될 때마다, 다른 모든 인스턴스로부터 분리된 자신만의 property값으로 설정됩니다. type property는 property를 타입 자체와 연결합니다. 얼마나 많은 타입 인스턴스를 만드는 것에는 상관없이 이 type property의 복사본을 갖게 됩니다.

특정 유형의 새 인스턴스를 생성할 필요 없이 액세스할 수 있으며, type property는 해당 타입의 어떠한 인스턴스에서도 사용가능합니다.

stored instance property와는 다르게 stored type property는 기본 값이 항상 주어집니다. 이는 타입 스스로가 초기화를 가질 수 없기 때문이며 초기화 하는 시간에 stored type property에서 값을 할당합니다.

사용할 키워드 측면에서 값 타입(구조 및 열거)과 참조 타입(클래스) 사이에는 작지만 유효한 차이가 있습니다. 값 타입에서 type property의 사용은 비교적 간단합니다.

```swift
struct SomeStructure {
    static var storedTypeProperty = "Some value."
    static var computedTypeProperty: Int {
        // return an Int value here
    }
}

enum SomeEnumeration {
    static var storedTypeProperty = "Some value."
    static var computedTypeProperty: Int {
        // return an Int value here
    }
}
```

참조 타입이라는 점에서 클래스의 type properties를 구현하면, 유형 속성이 하위 클래스에서 재정의되지 않을 경우 static 키워드를 사용하고, 해당 하위 클래스에서 type properties를 재정의할 경우 class 키워드를 사용해야 합니다.

더욱이 class 키워드를 사용한다는 것은 subclass에서 바뀔 가능성을 내포함으로 stored property로서 사용할 수 없습니다. (only computed)

또 다른 중요한 점으로는, 클래스 내의 static property는 subclass에서 overridden할 수 없지만 subClass에서 사용 가능합니다.

이는 아래의 예시에서 확인 가능합니다.

```swift
class Triangle {
    static var sides = 3

    class var sideToBeOverridden: Int {
      return 3
    }
}

var TriangleSide = Triangle.side // 3
var TriangleSide2 = Triangle.sideToBeOverridden // 3

Class Octagon: Triangle {
    override class var sideToBeOverridden: Int {
      return 8
    }
}

var OctagonSide = Octagon.side // 3
var OctagonSide2 = Octagon.sideToBeOverridden // 8
```


## Property observer

Property Observer는 lazy 저장 프로퍼티를 제외하고, stored property에 달 수 있는 것으로, 값의 변화를 주시하여 값이 변하기 직전(willSet)과 직후(didSet)에 어떤 행동을 할 수 있게 해주는 것입니다.

또한, 하위 클래스 내의 프로퍼티를 재정의하여, 상속된 프로퍼티(저장프로퍼티 or 연산프로퍼티 어느것이든)에도 프로퍼티 옵저버를 추가할 수 있습니다.

● willSet - 값이 저장되기 직전에 호출됩니다.

● didSet - 새로운 값이 저장된 직후에 호출됩니다.

```swift
class StepCounter {
    var totalSteps: Int = 0 {
        willSet(newTotalSteps) {
            print("About to set totalSteps to \(newTotalSteps)")
        }
        didSet {
            if totalSteps > oldValue  {
                print("Added \(totalSteps - oldValue) steps")
            }
        }
    }
}
let stepCounter = StepCounter()
stepCounter.totalSteps = 200
// About to set totalSteps to 200
// Added 200 steps
stepCounter.totalSteps = 360
// About to set totalSteps to 360
// Added 160 steps
```

### Reference:

- https://medium.com/@micosmin/swift-type-properties-5a51c50c00b7

- https://hcn1519.github.io/articles/2017-03/swift_property

- https://zeddios.tistory.com/243?category=685736

- https://medium.com/ios-development-with-swift/%ED%94%84%EB%A1%9C%ED%8D%BC%ED%8B%B0-get-set-didset-willset-in-ios-a8f2d4da5514
