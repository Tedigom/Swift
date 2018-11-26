# 분기문 conditional

분기문은 조건문이 참일 때 해당 코드를 실행하고, 거짓이면 다음으로 넘어가라는 의미를 지닙니다.

## if else 분기문

if 조건문 {참일 때 실행} else {거짓일 때 실행}

```swift
let temperature = 90
if temperature <= 32 {
  // code
} else if temperature >= 86 {
  // code
} else {
  // code
}
```

## 한줄 if문 Ternary operator

조건문 ? 참일 때 값: 거짓일 때 값

```swift
let isFemale = false

let name = isFemale ? "Lady" : "Tramp" // "Tramp"
```

## Nil-coalescing operator

a ?? b (Ternary operator로 표현한다면 a != nil ? a! : b)

```swift
var num: String = self.anotherNum ?? 90
```

## Switch - case 분기문

switch 판별대상 변수 { case 조건1: 코드 ... }

```swift
let someChar = "z"

switch someChar {
  case "a":
    print("a")
  case "b":
    print("b")
  default:
    print("no idea")
}
// no idea
```

## case - where

where은 case에 추가 조건을 주고자 할 때 쓰입니다.

```swift
let myInfo = ("John", 33)

switch myInfo {
  case ("Mike", 20), ("John", 20): // or을 콤마로 표현
    "first case"
  case (John, let age) where age == 33:
    "second case"
  default:
    break
}
// second case
```

## guard let-else

만일 다음 조건이 참이라면 다음 코드줄로 넘어갈 수 있다는 것을 말합니다. (다음 줄로 넘어갈 권한을 주는 특수한 if 문)

```swift
guard let name = person["name"] else { return }
```

## API 확인

 다른 사람이 만든 코드가 내가 개발하고자 하는 아이폰 버전에 부합하는 지 체킹할 필요가 있을 때 사용합니다.

 ```swift
if #available(iOS 10, macOS 10.12, *) {
   // code
} else {
   // code
}
 ```

## 이 외로 알아두어야할 것

- break

- continue

- fallthrough


### Reference:

 - https://m.blog.naver.com/jdub7138/220924373892
