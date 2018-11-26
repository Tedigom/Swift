# Value vs. Reference types

Reference type과 Value type은 개념적으로 다음과 같이 메모리에 저장된다고 볼 수 있습니다.
(메모리 할당에 대한 자세한 내용은 [여기](../additional_topics/allocation.md)를 참조하시면 됩니다.)

<img src = "./assets/val_ref.png">


## Value type

Value type은 변수에 값을 할당 하거나, 함수에서 호출할 때, 새로운 인스턴스를 만드는(copy) 데이터 type을 말합니다. Swift Named Types 중 enum과 struct는 Value type입니다.

값타입의 가장 큰 특징은 복사 — 대입, 초기화, 파라미터로 전달 –가 되어 개별 이스턴스들이 각각의 독립된 사본으로 존재하게 되는 것입니다.

ex) primitive data type - Int, Double, Char ...

<img src = "https://koenig-media.raywenderlich.com/uploads/2015/11/value_type.png" width = 150>

구조체와 열거형은 값 타입입니다. 그래서 기본적으로 인스턴스 매소드 내에서 값 타입의 프로퍼티를 변경할 수 없습니다. 때문에 값 타입 형의 매소드에서 프로퍼티를 변경하고 싶은 특정 상황이 존재한다면, 매소드에 mutating을 붙여 주면 가능합니다.

## Reference type

Reference type은 생성자를 통해 초기화 되고 나면, 변수에 값을 할당 하거나, 함수에서 호출할 때, 동일한 인스턴스의 reference(주소)를 반환하는 데이터 type을 말합니다. Swift Named Types 중 class는 Reference type입니다.

레퍼런스를 복사하는 것은 암시적으로는 공유된 인스턴스를 갖는 것입니다. 따라서 복사 후에도 두 인스턴스는 동일한 데이터이며, 두 번째 인스턴스를 변경하는 것은 원본에도 영향을 줍니다.

ex) Object


<img src = "https://koenig-media.raywenderlich.com/uploads/2015/11/reference_type.png" width = 190>

## 차이

<img src="./assets/coffee.gif">

> class는 변수 자신이 자신의 속성을 바꾸는 것 이외에도 외부에서 속성을 변경할 수 있습니다. 반면 struct는 자신의 속성은 자신이 바꾸어야 합니다. 그렇기 때문에 value type인 struct가 class보다 좀 더 mutation에 대해 안전하다고 할 수 있습니다.

이에 대한 예시입니다.


```swift
class SportsCar {
    var brand: String
    var model: String

    init(brand: String, model: String) {
        self.brand = brand
        self.model = model
    }
}

struct Truck {
    var weight: Int
    var mileage: Double
}

var sportCar1 = SportsCar(brand: "포르쉐", model: "911")
var sportCar2 = sportCar1
sportCar2.brand = "람보르기니"

print(sportCar1.brand) // "람보르기니" 출력 - Reference type 특징

var truck1 = Truck(weight: 2000, mileage: 16)
var truck2 = truck1
truck2.weight = 4000

print(truck1.weight) // 2000 출력 - Value type 특징
```

여러 Data type을 Value / Reference type으로 분류하면 다음과 같이 분류할 수 있습니다.

<img src = "https://cdn-images-1.medium.com/max/1600/1*6aJyC6_MrCRjdIAgXxAxkQ.png" width = 450>

## 변경과 안정성

레퍼런스 타입보다 값 타입을 사용하는 가장 주된 목적은 코드를 이해하기 쉽다는 것입니다. 각 인스턴스가 독립된 데이터 사본을 가지고 있다는 것은, 보이지 않는 곳에서 해당 데이터가 임의로 변경되지 않음을 보장할 수 있다는 것입니다. 특히 이는 멀티스레드 환경에서 다른 스레드가 예기치 못한 시점에 데이터를 변경해놓을 수 있는 것을 방지합니다. (보통 이런 류의 문제는 디버깅이 극히 어렵습니다.) 변경이 없으면 값타입이나 참조타입은 기본적으로 동일합니다. 값타입은 동기화 없이 스레드 간에 데이터를 전달할 수 있습니다. 따라서 안정성을 높이는 관점에서 바라볼 때 이런 모델은 코드를 보다 더 예측가능하게 만들어 줄 것입니다.

### Reference:

- https://www.raywenderlich.com/1314-getting-to-know-enums-structs-and-classes-in-swift

- http://dsnight.tistory.com/50

- https://soooprmx.com/archives/5355
