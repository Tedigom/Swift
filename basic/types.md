# Swift Type System

<img src = "https://koenig-media.raywenderlich.com/uploads/2015/11/types-700x195.png" width = 500>


스위프트의 타입에는 6개의 종류가 존재합니다. 4개의 named types (protocol, enum, struct, class) 와 2개의 compound type (tuple, function)이 스위프트의 6가지 type입니다. Basic type이라고 말해지는 Bool, Int, UInt, Float, Double, Character, String, Array, Set, Dictionary, Optional 등은 모두 named types로부터 만들어져 Swift Standard Library로 전달되는 것 입니다.

## Named Types

**Named types는 정의되었을 때 특정한 이름이 주어지는 타입입니다.** 예로 MyClass라고 이름지어진 User-defined class 의 instance는 MyClass 타입을 갖게 됩니다. 위에서 언급하였다시피, user-defined named types 에 추가적으로 Swift Standard Library 내에서는 Structures를 사용하여 array, dictionaries, optional values 등을 나타내기 위해 named types를 사용합니다. 이러한 것들은 named types이기 때문에 특정 behavior 등을 extend하기 위하여 extension declaration을 사용할 수 있습니다.

추가적으로 Named Type의 모든 모델(enum, struct, class)들은 프로토콜을 도입할 수 있습니다.

- [프로토콜](./protocol.md)

- [프로토콜 지향 프로그래밍](../features/pop.md)


<img src = "https://koenig-media.raywenderlich.com/uploads/2015/11/protocol.png" width = 250>


## Compound Types

**Compound types는 이름이 없는 Swift 언어 자체에서 정의되는 타입입니다.** function, tuple 2가지의 타입이 존재합니다. Compound type은 Named types나 혹은, 다른 compound types를 포함합니다. 예를 들어서 튜플 (Int, (Int, Int)) 타입의 경우는 Named type인 Int를 첫번째 요소로, Compound type인 (Int, Int)를 두번째 요소로 갖는 compound type 입니다.

추가로 named Type과 compound Type을 괄호로 묶을 수 있지만, 어느 "한"타입에 괄호를 묶는것은 아무런 효과가 없습니다. 예를 들어 (Int)와 Int는 동일한 타입입니다.


### Reference:

- https://www.raywenderlich.com/1314-getting-to-know-enums-structs-and-classes-in-swift

- https://docs.swift.org/swift-book/ReferenceManual/Types.html#//apple_ref/swift/grammar/type
