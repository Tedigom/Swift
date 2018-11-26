# 반복 Loop

특정 코드가 반복적으로 수행될 수 있도록 합니다.

## for in

``` swift
for i in 1..<4 {
  print(i)
}
// 1 2 3
```

## while

조건문이 참일 때 코드를 반복

``` swift
var number = 0

while number < 10 {
  print(number)
  number += 1
}
```

## repeat - while

조건과 관계없이 한번은 돌린 다음 조건문이 참이면 코드를 반복

``` swift
var number = 0

repeat {
  print(number)
  number += 1
} while number < 10
```

## for each

자세한 것은 [고차함수](../features/higher_order_functions.md) 문서 참조

for-in loop 같은 역할을 해줍니다. 아래는 array 안에 있는 1, 2, 3을 차례로 출력해줍니다.

```swift
[1, 2, 3].forEach { (each: Int) in
    print(each)
}
