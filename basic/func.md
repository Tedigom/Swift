# Function

## 함수의 정의와 호출

함수를 정의할 때는, 함수가 input으로 받을 값의 타입과 이름, 즉 parameters를 하나 또는 그 이상으로 정의할 수 있으며, 함수의 실행이 끝났을 때 돌려줄 값의 타입, 즉 return type을 정의할 수 있습니다. 모든 함수는 이름을 가지며, 함수의 이름은 해당 함수가 수행하는 일을 잘 설명할 수 있어야 합니다. 함수 내에서 return 을 하면 그 즉시 함수의 실행이 종료되고 리턴값이 반환됩니다.

```swift
func greet(person: String) -> String {
   let greeting = "Hello, " + person + "!"
   return greeting
}

print(greet(person: "Anna")) // prints "Hello, Anna!" print(greet(person: "Brian")) // prints "Hello, Brian!"

func greetAgain(person: String) -> String {
  return "Hello again, " + person + "!"
}

print(greetAgain(person: "Anna")) // prints "Hello again, Anna!"
````

## 함수의 인자

#### 복수 파라미터

```swift
func halfOpenRangeLength(start: Int, end: Int) -> Int {
    return end - start
}
```

#### 파라미터 없는 함수

```swift
func sayHelloWorld() -> String {
    return "hello, world"
}
```

#### Specifying Argument Labels

함수의 파라미터는 argument label 과 parameter name 을 가집니다.

- argument label은 함수를 호출할 때 arguments에 이름을 지정하기 위해 사용됩니다.

- parameter name 은 함수 내부 구현 안에서 사용이 됩니다.

따로 정의하지 않는 경우 parameter name과 argument label은 같은 이름을 사용하게 됩니다. argument label은 함수를 명시적이고 말이 되게끔 호출할 수 있게 해주는 데에 목적이 있으며, parameter name은 함수 내부 구현을 가독성있고 명확하게 보일 수 있게 해줍니다.


```swift
func someFunction(argumentLabel parameterName: Int) {
  // 밖에서 function call을 할 때는
  // argumentLabel이라는 이름에 argument를 넘기고
  // function body 에서는
  // parameterName이라는 이름을 사용하여 구현을 한다
}
```

#### Omitting Argument Labels

함수를 호출할 때 argument label을 명시하지 않게 하고 싶다면 언더바( _ )로 argument label을 정의할 수 있습니다.

```swift
func someFunction(_ firstParameterName: Int, secondParameterName: Int) {
   // function body안에서는
   // firstParameterName과 secondParameterName 사용
}

someFunction(1, secondParameterName: 2)
```

#### Default Parameter Values

모든 함수 파라미터는 디폴트 값을 설정할 수 있습니다. 디폴트 값이 정의된 파라미터는 함수 호출 시 생략할 수 있습니다.

```swift
func join(string s1: String, toString s2: String,
    withJoiner joiner: String = " ") -> String {
        return s1 + joiner + s2
}

join(string: "hello", toString: "world") // returns "hello world"
```

#### 가변 인자 (Variadic Parameters)

가변 인자는 특정 타입의 0 이상의 값을 받아 들입니다. 가변 갯수의 파라미터를 사용함으로써 함수 호출시 입력 값들이 임의의 갯수가 될수 있다고 정할 수 있습니다. 가변 인자 타입 이름 뒤에 마침표 세개(...)를 추가하여 가변 인자를 작성합니다.

```swift
func arithmeticMean(numbers: Double...) -> Double {
    var total: Double = 0
    for number in numbers {
        total += number
    }
    return total / Double(numbers.count)
}
arithmeticMean(1, 2, 3, 4, 5)
// returns 3.0, which is the arithmetic mean of these five numbers
arithmeticMean(3, 8.25, 18.75)
// returns 10.0, which is the arithmetic mean of these three numbers
```

#### 상수 인자와 변수 인자(Constant and Variable Parameters)

함수 인자는 기본적으로 상수입니다. 함수 내에에서 함수 인자 값을 변경하려고 한다면 컴파일 타임 에러가 발생합니다.

때로는 작업 중에 인자의 값을 변수에 복사하여 사용하기에 유용합니다. 하나 이상의 변수 인자를 함수에 사용하여 새로운 변수를 정의하는 것을 피할 수 있습니다.

변수 인자에 변화는 함수가 호출된 후에는 남지 않으며 함수 밖에서는 보이지 않습니다. 변수 인자의 생명주기는 함수가 호출되는 동안에만 존재합니다.

인자 앞에 var 키워드를 붙여 변수 인자를 정의합니다.

```swift
func alignRight(var string: String, count: Int, pad: Character) -> String {
    let amountToPad = count - countElements(string)

    if amountToPad < 1 {
        return string
    }
    
    let padString = String(pad)

    for _ in 1...amountToPad {
        string = padString + string
    }

    return string
}

let originalString = "hello"
let paddedString = alignRight(originalString, 10, "-")

// paddedString is equal to "-----hello"
// originalString is still equal to "hello"
```

#### 입출력 인자(In-Out Parameters)

앞에서 설명한 가변 인자는 함수 내에서만 변경 가능합니다. 만약 인자 값이 변경된 후에도 값이 유지되길 원한다면 인자를 입출력 인자로 정의해야 합니다.

입출력 인자는 inout 키워드가 인자 앞에 위치하도록 정의합니다. 입출력 인자는 함수로 넘겨진 값을 가지며, 함수에 의해 수정되고 원래 값을 대체하여 밖으로 넘겨집니다.

값을 변경할 수 없는 상수는 입출력 인자로 넘길 수 없으며, 변수만 가능합니다.

```swift
func swapTwoInts(inout a: Int, inout b: Int) {
    let temporaryA = a
    a = b
    b = temporaryA
}

var someInt = 3
var anotherInt = 107
swapTwoInts(&someInt, &anotherInt)

// someInt is now 107, and anotherInt is now 3
```

## 함수의 반환값


#### 다중 값을 반환하는 함수

```swift
func minMax(array: [Int]) -> (min: Int, max: Int) {
    var currentMin = array[0]
    var currentMax = array[0]
    for value in array[1..<array.count] {
        if value < currentMin {
            currentMin = value
        } else if value > currentMax {
            currentMax = value
        }
    }
    return (currentMin, currentMax)
}
```

#### 값을 반환하지 않는 함수

```swift
func sayGoodbye(personName: String) {
    print("Goodbye, \(personName)!")
}
```

#### 옵셔널 튜플 반환 타입

```swift
func minMax(array: [Int]) -> (min: Int, max: Int)? {
    if array.isEmpty { return nil }
    var currentMin = array[0]
    var currentMax = array[0]
    for value in array[1..<array.count] {
        if value < currentMin {
            currentMin = value
        } else if value > currentMax {
            currentMax = value
        }
    }
    return (currentMin, currentMax)
}
```

## 함수의 타입

더 자세한 내용은 [일급 객체](../features/first_class_functions.md)를 참고할 수 있습니다.

#### 함수 타입 사용

Swift의 다른 타입들 처럼 사용합니다. 예를 들어 함수 타입을 변수나 상수에 할당할 수 있습니다.

```swift
var mathFunction: (Int, Int) -> Int = addTwoInts
```

#### 인자 타입으로서 함수 타입

```swift
func printMathResult(mathFunction: (Int, Int) -> Int, a: Int, b: Int) {
    print("Result: \(mathFunction(a, b))")
}

printMathResult(addTwoInts, 3, 5)
// prints "Result: 8"
```

#### 반환 타입으로서 함수 타입

```swift
func stepForward(input: Int) -> Int {
    return input + 1
}

func stepBackward(input: Int) -> Int {
    return input - 1
}

func chooseStepFunction(backwards: Bool) -> (Int) -> Int {
    return backwards ? stepBackward : stepForward
}
```


## 중첩함수

함수 내부에서 또다른 함수를 정의할 수 있으며 이를 중첩 함수라고 합니다.

중첩 함수는 기본적으로 밖에서는 숨겨져 있으며 중첩 함수 중 하나를 반환하여 다른 범위에서 함수가 사용할 수 있게 합니다.

```swift
func chooseStepFunction(backwards: Bool) -> (Int) -> Int {
    func stepForward(input: Int) -> Int { return input + 1 }
    func stepBackward(input: Int) -> Int { return input - 1 }
    return backwards ? stepBackward : stepForward
}
```


### Reference:

- http://wlaxhrl.tistory.com/39
