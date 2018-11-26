# Function Type

Function type은 function, method, closure를 나타내는 타입이며 특정 기능을 하는 코드를 특정 방법으로 묶어낸 것입니다. 이러한 Function type은 반복적인 코딩을 피하기 위하여 사용하곤 합니다. Function type은 화살표( ->)로 구분되어지며 **파라미터 타입과 리턴 타입**으로 구성되어 있습니다.

- (parameter type) -> return type

#### parameter type:

입력값으로서 function type은 이러한 값들을 받아 특정 계산을 수행합니다. parameter type은 comma로 구분되어지는 타입 리스트입니다.

#### return type:

도출되는 특정 계산의 결과값입니다. return type은 튜플 타입을 지원하기 때문에 multiple values를 지원합니다.

Swift내에서 이러한 함수 타입은 [일급 객체](../features/first_class_functions.md)로서 다음의 사항들을 만족합니다.

1. 변수나 상수에 할당 가능

2. 인자값 (parameter)로 사용 가능

3. 반환타입 (return)으로 사용 가능


### Reference:

- http://wlaxhrl.tistory.com/39

- https://docs.swift.org/swift-book/ReferenceManual/Types.html#//apple_ref/swift/grammar/type

- http://seorenn.blogspot.com/2014/06/swift-function.html
