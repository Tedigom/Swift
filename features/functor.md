# Functor

결론적으로 functor는 어떤 value를 **map 함수를 지원하는 context로 쌀 수 있는 어떤 자료형태 (container type)** 를 말합니다.

map 함수를 사용하면 작업의도를 더 명확하게 표현할 수 있습니다. 즉, 어떻게가 아니라 무엇을 달성하려는지에 대해 더 잘 표현하는데 함수형 프로그래밍의 장점 중 하나라고도 할 수 있습니다. 아래는 그러한 장점의 예시입니다.

먼저, 루프를 사용하여 단순히 array element를 2배로 곱하는 예제입니다.

```swift
var list: [Int] = [1,2,3]
for element in list {
  list.append(element * 2)
}
print(list) // 2,4,6
```
이를 map 함수를 사용하면 다음과 같이 처리할 수 있습니다.

```swift
var list: [Int] = [1,2,3]
let doubledNumbers = list.map { $0 * 2 }
print(doubledNumbers) // 2,4,6
```

map은 array 뿐만 아니라 어떠한 컨테이너 타입에도 구현할 수 있는 고차 함수입니다. 여기에는 값이 있거나 또는 없음을 포장하는 Optional 타입도 해당합니다.

```swift
let number: Int? = 100 // Optional(100)
let transformedNumber = number.map{ $0 * 2 }
print(transformedNumber.map{ $0%2 == 0 }) // Optional(true)
```
Optional.map은 nil에 대한 처리를 보장해줍니다. 이를 통해 연산의 중간과정에서 중첩된 if let 혹은 guard let을 사용해야 하는 번거로움을 피할 수 있습니다.

```swift
let number: Int? = nil
let transformedNumber = number.map{ $0 * 2 }
print(transformedNumber.map{ $0%2 == 0 }) // Optional(nil)
```

### Reference:
- http://hiddenviewer.tistory.com/275
