# Method

Function과 Method는 모두 특정 작업을 수행하기 위한 독립적인 코드들의 집합이라는 점에서 동일하지만, 약간의 차이가 존재합니다. Method는 클래스, 구조체, 열거형 등의 객체에 종속적이지만, Function은 그렇지 않고 객체에 독립적입니다. 아래는 Function과 Method의 예시입니다.

```swift
func someFunction {
   //some code
}

class someClass {
  func someMethod {
    //some code
  }
}
```

Method는 항상 data type에 종속되는 만큼, Function에게는 없는 self라는 개념이 존재합니다. self는 Method가 호출된 객체를 지칭합니다.
