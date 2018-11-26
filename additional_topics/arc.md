# ARC (Automatic Reference Counting)

## ARC란?

Swift에서는 메모리를 관리할 때 ARC(Automatic Reference Counting)라는 메모리 관리 전략을 사용합니다. 메모리 관리는 아래의 영역 중 힙 영역과 관련됩니다. 힙 영역은 참조(Reference)형 자료들이 머무르는 공간이자 개발자가 동적으로 할당하는 메모리 공간으로서 관리가 필요한 영역입니다. Heap은 대표적으로 Swift의 참조형 자료인 클래스, 클로져 등의 자료가 머무르는 공간입니다. ( Swift의 value type은 ARC의 메모리 관리 대상이 아닙니다.) 이 **힙 영역의 참조형 자료들이 프로그램 상에서 얼마나 참조되는지 숫자를 세어서 (Reference Counting) 메모리에 자동으로 (Automatic) 할당 및 제거하도록 관리**하는 것이 ARC (Automatic Reference Counting) 입니다.

프로그램 메모리 할당에 관련된 내용은 [여기](./allocation.md)를 참조하시면 됩니다. 

<img src="https://t1.daumcdn.net/cfile/tistory/255B4A415835396538">

## How ARC Works

ARC는 클래스 인스턴스를 생성하였을 때, 메모리를 할당합니다. 그리고 클래스 인스턴스가 더이상 필요하지 않을 때, ARC는 해당 메모리를 해제합니다. 이 때 ARC는 잘못된 메모리 접근을 막기 위해(해제된 메모리에 접근하는 경우, 사용중인 메모리를 덮어씌워버리는 경우) 얼마나 많은 properties, 상수, 변수들이 각각의 클래스 인스턴스를 참조하고 있는지 숫자(reference count)를 셉니다. Swift에서는 각 객체마다 retain count를 가지고 있고, 이 count 수를 통해 메모리가 해제되어야 하는지를 결정합니다. 그래서 reference count가 0이 되는 순간까지 ARC는 객체를 메모리에서 해제하지 않습니다.

## Strong Reference

Swift에서 발생하는 Reference는 기본적으로 Strong Reference로 처리됩니다. 즉, let myClass = MyClass()와 같이 어느 클래스의 인스턴스를 선언하여 Reference가 발생하였을 때, 해당 인스턴스의 Reference Count가 0이 될 때까지 Swift가 해당 인스턴스를 메모리에 강하게 (Strong) 유지시켜주게 됩니다.

## Strong Reference Cycle

ARC는 개발자가 메모리 관리를 직접 하지 않도록 도와주는 좋은 메커니즘이지만, 몇 가지 경우에서 주의가 필요합니다. 그 중 가장 대표적인 케이스가 Strong Reference Cycle(강한 상호 참조)입니다. 강한 상호 참조는 2개의 reference type 데이터가 서로를 strong 인스턴스로 참조하고 있어서 reference count가 0이 되지 않는 상황을 의미합니다. 즉, 이들 인스턴스는 자신을 선언한 변수가 nil이 되어도 서로에 대한 상호 참조 때문에 메모리를 빠져나가지 못하게 되고, 이로 인해 메모리 누수 및 낭비가 일어나게 됩니다. 이를 Memory Cycle이라고도 부릅니다. 아래는 대표적 예시입니다.

```swift
class Person {
    var house: House?
    deinit {
        print("Remove Person")
    }
}

class House {
    var person: Person?
    deinit {
        print("Remove House")
    }
}

// 1
var person: Person? = Person() // 인스턴스 생성 후 다음 값 출력시 CFGetRetainCount(person) -> 2 출력(기본 count)
var house: House? = House()

// 2
person?.house = house
house?.person = person

// 3
person = nil
house = nil
```

위의 코드는 강한 상호 참조를 보여주는 예시입니다. person, house 인스턴스를 생성하고 모두 nil 처리를 해주었음에도 불구하고, deinit에 있는 print문은 호출되지 않았습니다. 즉, person.house와 house.person 모두 해제가 되지 않았습니다. reference count 메커니즘을 통해 생각해보면 이 문제의 답을 알 수 있습니다.

1. person과 house 인스턴스를 각각 생성하였고, 각각의 인스턴스는 strong reference count가 1이 됩니다.

2. person.house와 house.person에 각각 house와 person을 할당하였기 때문에 두 인스턴스의 strong reference count는 2가 되었습니다.

3. person과 house 인스턴스를 nil처리 했기 때문에 두 인스턴스의 strong reference count는 1이 됩니다. 그리고 strong reference count가 0이 되지 않았기 때문에 메모리 해제는 일어나지 않습니다.

위의 과정에서 발생한 가장 큰 문제는 인스턴스의 **메모리가 해제되지 않았는데 이에 접근할 방법이 없다는 것**입니다. 즉, person.house는 메모리에 남아 있는데, person 자체가 없어져버렸기 때문에 이에 접근이 불가능합니다. 이러한 상황을 메모리 누수(memory leak)가 발생했다고 말하고, 이는 반드시 고쳐야 합니다.

## Strong Reference Cycle 없애기

### 1) Weak

강한 상호 참조를 없애는 핵심 원리는 strong reference count를 세지 않는 것입니다. weak 을 사용하여 property를 선언하게 되면 해당 인스턴스에 대해서는 strong reference count를 카운팅하지 않고, **weak reference count**를 카운팅합니다. 이를 Swift 공식 문서에서는 weak reference가 property에 대해 strong hold를 하지 않는다고 표현합니다.

<img src = "https://dl.dropbox.com/s/ct7p10tglou2zkh/스크린샷%202018-07-07%20오후%207.29.35.png" width = 600>

위에서 사용한 예제를 weak을 통해 수정해보겠습니다.

```swift
class Person {
    weak var house: House?
    deinit {
        print("Remove Person")
    }
}

class House {
    weak var person: Person?
    deinit {
        print("Remove House")
    }
}

// 1
var person: Person? = Person()
var house: House? = House()

// 2
person?.house = house
house?.person = person

// 3
person = nil
house = nil

// Remove Person
// Remove House
```

1. 앞선 예제와 동일하게 인스턴스를 생성하여 strong reference count가 1이 되었습니다.

2. person.house, house.person 모두 weak reference이기 때문에 참조 값은 할당

3. strong reference count가 올라가지 않고, weak reference count가 증가합니다.

4. ARC는 strong reference count를 통해 메모리 해제를 수행하기 때문에 person = nil을 수행하면(메모리에서 해제하면), weak reference도 모두 메모리에서 해제합니다.

이번에는 앞선 예제와 다르게 deinit이 호출되었습니다. 모든 변수가 문제 없이 메모리에서 해제된 것입니다. 이는 property 앞에 명시된 weak을 키워드로 인해 strong reference count가 올라가지 않았기 때문입니다. 즉 person = nil을 통해서 strong reference count가 0이 되어 person의 메모리가 해제되면서 그 안의 weak으로 설정된 객체인 house를 함께 메모리에서 해제한 것입니다.

이와 같은 방식으로 strong reference cycle은 weak을 사용하면 strong reference cycle을 해결할 수 있습니다.

다만, Weak reference는 ARC를 통해 런타임에서 nil이 될 수 있어야 하기 때문에 항상 **옵셔널 타입**으로 선언되고 상수가 아닌 변수여야 합니다.

일반적으로 개념상, 상대보다 프로그램에 더 오래 머무르는 클래스 안에서 선언하는데, 이는 내 안에서 상대를 nil해버리는 것이므로 내가 더 오래 남아 있는 경우이기 때문입니다.

### 2) Unowned

unowned 즉, 미소유 참조는 해당 인스턴스에 대하여 Reference Counting을 하지 말라고 선언하는 것 입니다. 아예 Reference Counting을 하지 않는 셈이니, Strong Reference Cycle이 발생할 여지도 없어집니다. 앞서 살펴본 weak (약한 참조) var는 참조하는 인스턴스가 nil이 될 수 있게 열어둠으로써 memory cycle을 피하는 것이라면, unowned (미소유 참조)는 아예 Reference Counting을 하지 않아 memory cycle을 피하는 방법입니다.

다만, 이렇게 Reference Counting을 하지 않는 상태에서 참조하는 인스턴스가 nil이 될 수 있는 여지를 주게 되면 원본이 사라져버린 참조가 발생할 위험이 존재합니다. 이 경우 프로그램 크래쉬가 발생합니다. 그러므로 unowned 선언하는 변수는 nil이 될 수 없기에 Optional 자료형이 될 수 없습니다. 이러한 특성에 따라 unowned는 자신이 상대방 없이 남아있을 수가 없으므로 weak var과 반대로 수명이 짧은 클래서 쪽에서 선언됩니다. 이를 통해 참조하는 인스턴스 원본이 사라질 때 자신도 같이 사라지게 설계되도록 주의를 기울여야 합니다.

이렇듯 어느 한쪽이 절대로 nil이 될 수 없는 특수한 케이스에만 unowned가 사용가능하기 때문에 일반적으로 weak var가 선호됩니다.

## Closure Memory Cycle

인스턴스 외에 클로져 역시 대표적인 Reference type 이므로 memory cycle이 발생할 수 있습니다. 클로저는 자신이 정의되는 시점에 주변 환경(문맥, context)으로부터 여러 상수나 변수에 대한 참조를 잡아내서(capture) 저장합니다. 이렇게 문맥에서 참조를 잡아서 저장하는 과정을 클로징(closing)이라고 하고, 상수와 변수에 대해 그렇게 닫기 때문에 클로저(closure)라 부릅니다. 아래는 이러한 클로져의 특성인 캡쳐를 보여주는 예시입니다.

```swift
func makeIncrementor(forIncrement amount: Int) -> () -> Int {
    var runningTotal = 0
    func incrementor() -> Int {
        runningTotal += amount
        return runningTotal
    }
    return incrementor
}
let incrementByTen = makeIncrementor(forIncrement: 10)
let incrementBySeven = makeIncrementor(forIncrement: 7)
incrementByTen() // 10 반환
incrementBySeven() // 7 반환
incrementByTen() // 20 반환
incrementByTen() // 30 반환
```
위 예에서처럼, incrementBySeven나 incrementByTen 자체는 상수(let으로 정의)였지만, 클로저에 잡혀있는 변수(runningTotal)는 호출할 때마다 변할 수 있습니다. 왜냐하면 incrementBySeven나 incrementByTen가 상수라서 가리키는 대상(참조)을 바꿀 수는 없지만, 그 대상 (클로저) 자체는 상태가 바뀔 수 있기 때문입니다.

즉, 클로져는 참조 타입이기 때문에, 클로저를 다시 다른 상수나 변수에 대입하면, 같은 대상 클로저를 가리키는 것입니다. 따라서 캡쳐하여 참조하는 값이 사라지면서 강한 상호 참조가 발생할 수 있습니다.

이처럼 참조 타입의 특성을 가지고 있는 클로저의 강한 상호 참조 역시 weak을 통해서 해결할 수 있습니다. 기본 원리는 위에서 클래스 간 강한 상호 참조를 없애는 방식과 동일합니다. 다만, 클로저는 주변 값을 capture할 때 weak으로 이 capture를 진행하면 됩니다. 방식은 다음과 같습니다.

```swift
lazy var printDescription: () -> Void = { [weak self] in
  guard let strongSelf = self else { return }
  print(strongSelf.description)
}
```

### Reference:
- https://m.blog.naver.com/jdub7138/220928509246
- https://hcn1519.github.io/articles/2018-07/swift_automatic_reference_counting
