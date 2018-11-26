# Struct (구조체)

구조체와 클래스는 프로그래머가 데이터를 용도에 맞게 묶어 표현하고자 할 때 용이합니다. 구조체와 클래스는 프로퍼티와 메소드를 사용하여 구조화된 데이터와 기능을 가질 수 있습니다. 하나의 새로운 사용자 정의 데이터 타입을 만들어 주는 것 입니다. 아래와 같이 구조체를 정의할 수 있습니다.

```swift
struct BasicInformation {
    var name: String
    var age: Int
}
```

구조체 정의를 마치고 구조체의 인스턴스를 생성하고 초기화할 수 있습니다. 구조체에 기본 생성된 이니셜라이저의 매개변수는 구조체의 프로퍼티 이름으로 자동 지정됩니다. 구조체를 상수로 선언하면 내부 프로퍼티 값을 변경할 수 없고, 변수로 선언하면 내부 프로퍼티 값을 변경해 줄 수 있습니다.

```swift
// 프로퍼티 이름(name, age)으로 자동 생성된 이니셜라이저를 사용하여 구조체를 생성합니다.
var aInfo: BasicInformation = BasicInformation(name: "a", age: 99)
aInfo.age = 100 // 변경 가능!
aInfo.name = "Bear" // 변경 가능!

// 프로퍼티 이름(name, age)으로 자동 생성된 이니셜라이저를 사용하여 구조체를 생성합니다.
let bInfo: BasicInformation = BasicInformation(name: "b", age: 99)
bInfo.age = 100 // 변경 불가!
```


### Reference:

- http://blog.yagom.net/530
