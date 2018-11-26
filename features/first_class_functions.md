# First Class Function (일급함수)

프로그래밍 언어에서 함수를 값으로 다룰수 있는 개념입니다. (함수 스스로 일급 객체 취급)

## First-class citizen (1급 객체)

아래 조건을 충족한다면 1급 객체라고 할수 있습니다.

1. 변수나 상수에 할당 할 수 있어야 한다.

2. 객체의 인자로 넘길 수 있어야 한다.

3. 객체의 리턴값으로 리턴 할수 있어야 한다.

4. 컴파일 시점이 아닌 런타임 시점에 생성이 가능해야 한다.

5. 할당에 사용된 이름과 관계없이 고유한 구별이 가능해야 한다


### 1. 변수나 상수에 함수를 할당

- 변수나 상수에 함수를 할당할 때, 함수는 실행되지 않고 객체만 할당됩니다.

- 함수 타입으로 변수나 상수를 선언 가능합니다.

- 인자값의 타입과 반환값에 관한 것만 표기합니다.

```swift
func foo(base: Int) -> String {
  return "결과값은 \(base+1)입니다."
}

let f : Int -> String = foo // 함수 타입 선언 및 할당
f(5) // 결과값은 6입니다.
```


### 2. 함수의 반환 타입으로 함수를 사용

```swift
func foo() -> String {
  return "this is foo()"
}

func boo() -> (Void -> String) {
  return foo
}

let b = boo()
b() // this is foo()
```
```swift
func plus(a: Int, b: Int) -> Int {
    return a + b
}

// 함수의 반환타입을 바탕으로 함수의 변수 할당
func calc(operand: String) -> (Int, Int) -> Int {
    switch operand {
    case "+":
        return plus
    default:
        return plus
    }
}

let c = calc(operand: "+")
c(3,4) // return 7
```

### 3. 함수의 인자값으로 함수를 사용

```swift
func boo(param: Int) -> Int {
  return param + 1
}

func foo(base: Int, function f: Int -> Int) -> Int {
  return f(base)
}

foo(3, function: boo) // return 4
=
```

### Currying

이러한 일급함수의 특성을 사용하여 Currying이란 기법을 구현할 수 있습니다. Currying은 여러개의 인자를 가진 함수를 호출 할 경우, 파라미터의 수보다 적은 수의 파라미터를 인자로 받으면 누락된 파라미터를 인자로 받는 기법을 말합니다.

```swift
func curry<X, Y, Z>(f: @escaping (X, Y) -> Z) -> (X) -> (Y) -> Z {
    return { x in
        { y in
            f(x, y)
        }
    }
}

func add(a: Int, b:Int) -> Int {
    return a + b
}

let result = curry(f: add)(3)(4)
```

curry(f: add)(3)(4)처럼 하나 씩 분리해서 받는 기법을 currying(커링) 이라고 합니다. 이러한 커링의 이점은 다음과 같습니다.

1. 여러 파라미터를 한번 에 받지않고 부분적으로 받은 후 함수의 실행을 늦출수 있습니다.(전체가 아닌 부분적으로 파라미터를 받는다고 해서 함수가 실행되지 않는다)

2. 변하지 않는 부분은 별도로 정의함으로써 간단하게 재사용 할 수 있습니다.


### Reference:
- http://blog.xenomity.com/entry/Functional-Programming-1급-함수와-고차-함수의-개념
- https://m.blog.naver.com/PostView.nhn?blogId=yukhyung&logNo=220801488911&proxyReferer=https%3A%2F%2Fwww.google.com%2F
