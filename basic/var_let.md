# 변수와 상수

## 변수 var

var는 variable의 약자로 변수를 말합니다. 참조 및 조작이 가능합니다. 변경 가능하다는 의미에서 mutable한 특성을 갖는다고 말합니다. 보통 자료형을 선언하고 값을 대입합니다.

```swift
var 변수명: 자료형 = 값
var centimeter: Double = 5.00
```

일반적으로 Swift는 매우 똑똑한 언어이기 때문에 변수의 자료형을 추론할 수 있습니다. (자료형 추론) 아래의 예시와 같이 Double이라고 자료형을 명시하지 않아도 언어 스스로 값을 보고 자료형을 추론하여 계산합니다.

```swift
var centimeter = 5.00
```

## 상수 let

상수는 항상 같은 수라는 의미입니다. 변수와 달리 변경이 불가능하여 immutable하다고 말합니다.

```swift
let centimeter: Double = 5.00
```

### Reference:

- https://m.blog.naver.com/jdub7138/220924072382
