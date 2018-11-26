# Functional Programming 이란

<img src="https://cdn-images-1.medium.com/max/1600/1*AM83LP9sGGjIul3c5hIsWg.png" width=700>

Wikipedia의 정의에 따르면, 다음과 같습니다.

> 자료 처리를 수학적 함수의 계산으로 취급하고 상태와 가변 데이터를 멀리하는 프로그래밍 패러다임

명령형의 함수는 프로그램의 상태의 값을 바꿀 수 있는 side-effect가 있어 같은 코드라도 실행되는 프로그램의 상태에 따라 다른 결과값을 낼 수 있지만, 함수형의 함수는 출력값은 함수에 입력된 인수에만 의존하므로 항상 동일한 결과가 나오게 됩니다.

때문에 명령형 함수의 side-effect를 제거하면 프로그램의 동작을 이해하고 예측하기가 훨씬 쉬워질 것입니다. 이 것이 함수형 함수의 핵심 동기입니다.

이런 부분을 보면 Swift에서 let과 value type이 차지하는 비중을 가늠해볼 수 있습니다.
(프로그램의 상태에 의존적이지 않으므로)

즉, 기본적인 개념은 함수형 언어가 아니라도 적용 가능한 부분이 많으며 Objective-C Cocoa Framework에서도 mutable/immutable로 분리된 객체는 바탕에 깔린 기본적은 철학은 함수형에서 말하는 동기와 일맥상통하는 면이 있습니다.

## Functional Programming Language의 기본이 되는 세가지 function #

> Swift는 아래의 세가지 모두를 가집니다. 그러므로 완전한 함수형 언어에 비하면 부족함이 있기는 하지만 하이브리드 언어로서 functional programming으로 나아가기 위한 기본적인 준비는 되어있다고 생각됩니다.

### 1) Swift에서의 Pure Function

side-effect가 없는 함수, 즉, 실행이 외부에 영향을 끼치지 않는 함수입니다. 따라서 thread에 안전하고 병렬적인 계산이 가능합니다.

매우 쉽게 구현 할 수 있습니다. 가장 기본적이며 다른 functional programming의 속성과 결합하여 강력한 도구가 될 수 있습니다.

```swift
func plus10(value: Int) -> Int {
    return value + 10
}
```

### 2) Swift에서의 Anonymous Function

이름이 없는 함수입니다. Swift의 closure나 Objective-C의 block 같은 것들이 이에 해당합니다.

```swift
let f = { (a: Int) in return a + 10 }
```

위의 코드 (클로저) 를 보면 정수 a를 받아서 10을 더해서 돌려주는 함수라는 것을 알 수 있습니다. 하지만, 함수의 이름은 없습니다. 다만 상수 f에 대입되어있을 뿐입니다.

(클로저의 경우, 단순 이름이 없을 뿐만 아니라 정의되는 시점에 캡쳐를 통하여 주변환경(context)으로부터 여러 상수와 변수의 값의 참조들을 저장하거나 값 자체를 복사하여 내부적으로 저장할 수있다는 특성도 가지고 있습니다.)

### 3) Swift에서의 Higher-order Function

함수를 다루는 함수입니다. 함수를 인자로 받거나 함수를 반환하는 함수입니다. 이것이 가능한 이유는 Functional Programming에서는 함수도 값으로 취급하기 때문입니다.


```swift
func run(completion: () -> Void) {
    completion()
}
```

run이라는 함수는 function(anonymous function)을 인자로 받고 있습니다. 이런 함수를 higher-order function이라 합니다. 위의 경우는 함수를 인자로 받는 경우이지만, 반대로 함수를 return 할 수도 있습니다.


### Reference:
- https://yobi.navercorp.com/iOSDev/posts/252
