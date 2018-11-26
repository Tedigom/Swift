# Naming Convention
> 본 문서는 https://github.com/raywenderlich/swift-style-guide 의 번역본입니다.

## Table of Contents

* [Naming](#naming)
  * [Prose](#prose)
  * [Delegates](#delegates)
  * [Use Type Inferred Context](#use-type-inferred-context)
  * [Generics](#generics)
  * [Class Prefixes](#class-prefixes)
  * [Language](#language)
* [Code Organization](#code-organization)
  * [Protocol Conformance](#protocol-conformance)
  * [Unused Code](#unused-code)
  * [Minimal Imports](#minimal-imports)
* [Spacing](#spacing)
* [Classes and Structures](#classes-and-structures)
  * [Use of Self](#use-of-self)
  * [Protocol Conformance](#protocol-conformance)
  * [Computed Properties](#computed-properties)
* [Function Declarations](#function-declarations)
* [Closure Expressions](#closure-expressions)
* [Types](#types)
  * [Constants](#constants)
  * [Static Methods and Variable Type Properties](#static-methods-and-variable-type-properties)
  * [Optionals](#optionals)
  * [Lazy Initialization](#lazy-initialization)
  * [Type Inference](#type-inference)
  * [Syntactic Sugar](#syntactic-sugar)
* [Functions vs Methods](#functions-vs-methods)
* [Memory Management](#memory-management)
  * [Extending Lifetime](#extending-lifetime)
* [Access Control](#access-control)
* [Control Flow](#control-flow)
* [Golden Path](#golden-path)
  * [Failing Guards](#failing-guards)
* [Semicolons](#semicolons)
* [Parentheses](#parentheses)

## Naming

- 간결함 보다 우선 순위를 명확하게
- Camel case 사용
- 타입과 프로토콜에서는 Uppercase, 나머지는 lowercase
- 불필요한 단어를 생략하고, 필요한 모든 단어는 포함
- 타입이 아닌 역할에 기반하여 네이밍
- 팩토리 메소드는 `make`로 시작
- 메소드가 끼치는 영향에 대한 네이밍 방법
  - non-mutating 동사 메소드의 경우, -ed 또는 -ing 사용
  - mutating 명사 메소드의 경우, formX 사용
  - boolean 타입은 assertions 처럼 읽어야 함
  - 무엇인가를 기술하는 protocol은 명사로 읽어야 함
  - 사용성을 기술하는 protocol은 -able 또는 -ible로 끝나야 함
- 전문가를 당황하게 하지않고, 초보자를 혼란스럽게 하지 않는 용어 사용
- 일반적으로 약어는 생략
- 이름에 대한 선례 사용
- free functions 보다 메소드, 프로퍼티 사용
- casing acronyms and initialisms uniformly up or down(?)
- 동일한 의미를 공유하는 메소드에는 같은 베이스 이름을 부여
- return 타입을 통한 오버로드 회피
- 문서로 사용되는 좋은 매개 변수 이름 선택
- 클로저와 튜플 파라미터에 라벨 지정
- 기본 매개변수 활용

### Class Prefixes

Swift 타입은 자동적으로 네임 스페이스를 포함하는 모듈에 의해 네임 스페이스가 지정되므로 RW와 같은 클래스 접두사를 추가하면 안됩니다. 만약 다른 모듈의 두 이름이 충돌하면 타입 이름 앞에 모듈 이름을 붙여 명확하게 할 수 있습니다. 단, 혼동이 발생할 가능성이 있는 경우에만 모듈 이름을 지정하십시오.

```swift
import SomeModule

let myClass = MyModule.UsefulClass()
```

### Delegates

사용자 지정 Delegate 메소드를 생성할 때에는 이름이 지정되지 않은 첫번째 매개 변수가 Delegate 시작지점이어야 합니다.

**Preferred:**
```swift
func namePickerView(_ namePickerView: NamePickerView, didSelectName name: String)
func namePickerViewShouldReload(_ namePickerView: NamePickerView) -> Bool
```

**Not Preferred:**
```swift
func didSelectName(namePicker: NamePickerViewController, name: String)
func namePickerShouldReload() -> Bool
```

### Use Type Inferred Context

컴파일러의 타입 추론을 사용하여 짧고 명료한 코드를 작성하십시오.

**Preferred:**
```swift
let selector = #selector(viewDidLoad)
view.backgroundColor = .red
let toView = context.view(forKey: .to)
let view = UIView(frame: .zero)
```

**Not Preferred:**
```swift
let selector = #selector(ViewController.viewDidLoad)
view.backgroundColor = UIColor.red
let toView = context.view(forKey: UITransitionContextViewKey.to)
let view = UIView(frame: CGRect.zero)
```

### Generics

일반적인 유형 매개 변수는 서술적인 UpperCamelCase 이름이어야 합니다. 이름에 의미 있는 관계나 역할이 없는 경우에는 "T", "U" 또는 "V"와 같은 전통적인 대문자를 사용하십시오.

**Preferred:**
```swift
struct Stack<Element> { ... }
func write<Target: OutputStream>(to target: inout Target)
func swap<T>(_ a: inout T, _ b: inout T)
```

**Not Preferred:**
```swift
struct Stack<T> { ... }
func write<target: OutputStream>(to target: inout target)
func swap<Thing>(_ a: inout Thing, _ b: inout Thing)
```

### Language

Apple의 API와 일치하도록 US영어 철자를 사용합니다.

**Preferred:**
```swift
let color = "red"
```

**Not Preferred:**
```swift
let colour = "red"
```

## Code Organization

extensions를 사용하여 코드를 논리적 기능 블록으로 구성합니다. 각각의 extension에는 "// MARK: -" 코멘트로 설정해줍니다.

### Protocol Conformance

특히, 모델에 대한 프로토콜 적합성을 추가할 때 프로토콜 메소드들에 대한 별도의 extension을 추가하는 것을 추천합니다. 이렇게 하면 관련 메소드가 프로토콜과 함께 그룹화되며 관련 메소드를 사용하여 프로토콜을 클래스에 추가하는 지침을 간소화할 수 있습니다.

**Preferred:**
```swift
class MyViewController: UIViewController {
  // class stuff here
}

// MARK: - UITableViewDataSource
extension MyViewController: UITableViewDataSource {
  // table view data source methods
}

// MARK: - UIScrollViewDelegate
extension MyViewController: UIScrollViewDelegate {
  // scroll view delegate methods
}
```

**Not Preferred:**
```swift
class MyViewController: UIViewController, UITableViewDataSource, UIScrollViewDelegate {
  // all methods
}
```

UIKit의 뷰 컨트롤러의 경우, 라이프사이클, 커스텀 accessories 및 IBAction은 별도의 클래스 extension을 고려하십시오.

### Unused Code

Xcode 템플릿 코드 및 Placeholder 코멘트를 포함한 미사용(dead)코드를 제거해야 합니다. 예외는 튜토리얼이나 설명서가 사용자에게 코멘트된 코드를 사용하도록 지시하는 경우입니다.

**Preferred:**
```swift
override func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
  return Database.contacts.count
}
```

**Not Preferred:**
```swift
override func didReceiveMemoryWarning() {
  super.didReceiveMemoryWarning()
  // Dispose of any resources that can be recreated.
}

override func numberOfSections(in tableView: UITableView) -> Int {
  // #warning Incomplete implementation, return the number of sections
  return 1
}

override func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
  // #warning Incomplete implementation, return the number of rows
  return Database.contacts.count
}

```
### Minimal Imports

import를 최소화하십시오. 예를 들어 `Foundation`으로 충분할 때 `UIKit`을 가져 오지 않아도됩니다.

## Spacing

* 메서드 중괄호와 다른 중괄호 (`if` /`else` /`switch` /`while` 등)는 항상 문장과 같은 줄에서 열리지 만 새로운 줄에서 닫도록 합니다.

**Preferred:**
```swift
if user.isHappy {
  // Do something
} else {
  // Do something else
}
```

**Not Preferred:**
```swift
if user.isHappy
{
  // Do something
}
else {
  // Do something else
}
```

* 콜론은 항상 왼쪽에는 공백이없고 오른쪽에는 공백이 없도록 합니다. 예외는 삼항 연산자 `?:`, 이름 없는 매개 변수`(_:)`, 비어있는 딕셔너리 `[:]` 및 `#selector` 구문입니다.

**Preferred:**
```swift
class TestDatabase: Database {
  var data: [String: CGFloat] = ["A": 1.2, "B": 3.2]
}
```

**Not Preferred:**
```swift
class TestDatabase : Database {
  var data :[String:CGFloat] = ["A" : 1.2, "B":3.2]
}
```

* 긴 줄은 약 70 자로 묶어야합니다.

* 줄 끝의 공백을 피하십시오.

* 각 파일의 끝에 하나의 개행 문자를 추가하십시오.

## Classes and Structures

### Which one to use?

구조체는 [값 타입](https://developer.apple.com/library/mac/documentation/Swift/Conceptual/Swift_Programming_Language/ClassesAndStructures.html#//apple_ref/doc/uid/TP40014097-CH13-XID_144)입니다. identity가 없는 것에 구조체를 사용하십시오. [a, b, c]가 포함된 배열은 [a, b, c]가 포함된 다른 배열과 실제로 동일하며, 완전히 바꿔 쓸 수 있습니다. 첫 번째 배열을 사용하든 두 번째 배열을 사용하든 관계없이 똑같은 것을 나타 내기 때문에 중요하지 않습니다. 이것이 배열이 구조체 인 이유입니다.

클래스는 [참조 타입](https://developer.apple.com/library/mac/documentation/Swift/Conceptual/Swift_Programming_Language/ClassesAndStructures.html#//apple_ref/doc/uid/TP40014097-CH13-XID_145)입니다. identity나 특정 라이프 사이클이 있는 것에 클래스를 사용하십시오.

### Example definition

Here's an example of a well-styled class definition:

```swift
class Circle: Shape {
  var x: Int, y: Int
  var radius: Double
  var diameter: Double {
    get {
      return radius * 2
    }
    set {
      radius = newValue / 2
    }
  }

  init(x: Int, y: Int, radius: Double) {
    self.x = x
    self.y = y
    self.radius = radius
  }

  convenience init(x: Int, y: Int, diameter: Double) {
    self.init(x: x, y: y, radius: diameter / 2)
  }

  override func area() -> Double {
    return Double.pi * radius * radius
  }
}

extension Circle: CustomStringConvertible {
  var description: String {
    return "center = \(centerString) area = \(area())"
  }
  private var centerString: String {
    return "(\(x),\(y))"
  }
}
```

위의 예는 다음과 같은 스타일 지침을 보여줍니다.

 + 프로퍼티, 변수, 상수, 인자 선언 후 콜론 다음에 공백을 넣은 후 타입을 지정합니다. `x: Int`, 그리고 'Circle: Shape`.
 + 공통 목적 / 상황을 공유하는 경우 여러 변수와 구조를 단일 행에 정의하십시오..
 + getter 및 setter 정의 및 프로퍼티 옵저버는 들여 씁니다.
 + 접근 지정자가 이미 디폴트인 경우는`internal` 등의 수식자를 추가하지 마십시오. 마찬가지로 메서드를 재정의 할 때 접근 지정자를 반복하지 마십시오.

### Use of Self

Swift는 객체의 속성에 접근하거나 메소드를 호출 할 필요가 없기 때문에 'self'를 사용하지 마십시오.
`@escapeping` 클로저 또는 이니셜라이저에서 프로퍼티를 명확하게하는 경우에만 사용하십시오. 다른 의미로, 'self'없이 컴파일 가능하면 생략합니다.

### Computed Properties

계산 된 프로퍼티가 읽기 전용이면 get 절을 생략하십시오. get 절은 set 절이 제공된 경우에만 필요합니다.

**Preferred:**
```swift
var diameter: Double {
  return radius * 2
}
```

**Not Preferred:**
```swift
var diameter: Double {
  get {
    return radius * 2
  }
}
```

## Function Declarations

여는 중괄호를 포함하여 한 줄에 짧은 함수 선언을 유지하십시오 :

```swift
func reticulateSplines(spline: [Double]) -> Bool {
  // reticulate code goes here
}
```
긴 시그니처를 포함하는 함수의 경우 적절한 위치에 줄 바꿈을 추가하고 후속 줄에 추가 들여 쓰기를 추가하십시오.

```swift
func reticulateSplines(spline: [Double], adjustmentFactor: Double,
    translateConstant: Int, comment: String) -> Bool {
  // reticulate code goes here
}
```

## Closure Expressions

인수 목록의 끝에 단일 클로저 표현식 매개 변수가있는 경우에만 후행 클로저 구문을 사용하십시오.

**Preferred:**
```swift
UIView.animate(withDuration: 1.0) {
  self.myView.alpha = 0
}

UIView.animate(withDuration: 1.0, animations: {
  self.myView.alpha = 0
}, completion: { finished in
  self.myView.removeFromSuperview()
})
```

**Not Preferred:**
```swift
UIView.animate(withDuration: 1.0, animations: {
  self.myView.alpha = 0
})

UIView.animate(withDuration: 1.0, animations: {
  self.myView.alpha = 0
}) { f in
  self.myView.removeFromSuperview()
}
```

컨텍스트가 명확한 단일 표현식 클로저의 경우에는 암시적 리턴을 사용하십시오:

```swift
attendeeList.sort { a, b in
  a > b
}
```

후행 클로저를 사용하는 체인화 된 메서드는 문맥에서 읽기 쉽고 명확해야합니다. 예:

```swift
let value = numbers.map { $0 * 2 }.filter { $0 % 3 == 0 }.index(of: 90)

let value = numbers
  .map {$0 * 2}
  .filter {$0 > 50}
  .map {$0 + 10}
```

## Types

가능한 경우 Swift의 기본 유형을 사용하십시오. Swift는 Objective-C에 브리징을 제공하므로 필요에 따라 전체 메소드 세트를 계속 사용할 수 있습니다.

**Preferred:**
```swift
let width = 120.0                                    // Double
let widthString = (width as NSNumber).stringValue    // String
```

**Not Preferred:**
```swift
let width: NSNumber = 120.0                          // NSNumber
let widthString: NSString = width.stringValue        // NSString
```

Sprite Kit 코드에서 너무 많은 변환을 피하면서 코드를보다 간결하게 만들려면`CGFloat`을 사용하십시오.

### Constants

상수는 `let` 키워드를 사용하여 정의되고, 변수는 `var` 키워드로 정의됩니다. 변수의 값이 변경되지 않으면 항상`var` 대신`let`을 사용하십시오.
타입 프로퍼티를 사용하여 해당 타입의 인스턴스가 아닌 타입에 상수를 정의 할 수 있습니다. 타입 프로퍼티를 상수로 선언하려면 `static let`을 사용하십시오. 이러한 방식으로 선언 된 유형 특성은 일반적으로 인스턴스 특성과 구별하기 쉽기 때문에 전역 상수보다 선호됩니다. 예:

**Preferred:**
```swift
enum Math {
  static let e = 2.718281828459045235360287
  static let root2 = 1.41421356237309504880168872
}

let hypotenuse = side * Math.root2

```
**Note:** case-less한 열거형을 사용하면 우연히 인스턴스화 될 수없고 순수한 네임 스페이스로 작동 할 수 있다는 장점이 있습니다.

**Not Preferred:**
```swift
let e = 2.718281828459045235360287  // pollutes global namespace
let root2 = 1.41421356237309504880168872

let hypotenuse = side * root2 // what is root2?
```

### Static Methods and Variable Type Properties

정적 메서드 및 타입 프로퍼티는 전역 함수 및 전역 변수와 유사하게 작동하며 아껴서 사용해야합니다. 기능이 특정 유형으로 범위를 지정하거나 Objective-C 상호 운용성이 필요한 경우 유용합니다.

### Optionals

`?`를 사용하여 변수 및 함수 반환 유형을 선택적으로 선언합니다 (nil 값을 사용할 수 있는 경우).

`viewDidLoad`에서 셋업 될 서브 뷰와 같이 나중에 사용하기 전에 초기화 될 인스턴스 변수에 대해서만`!`로 선언 된 암시적으로 언래핑 된 타입을 사용하십시오.

옵셔널 값에 접근 할 때 값이 한 번만 액세스되거나 체인에 많은 옵셔널이있는 경우 체인을 사용하십시오.

```swift
self.textContainer?.textLabel?.setNeedsDisplay()
```

한 번 래핑을 해제하고 여러 작업을 수행하는 것이 더 편리 할 때 옵셔널 바인딩을 사용하십시오:

```swift
if let textContainer = self.textContainer {
  // do many things with textContainer
}
```
옵셔널 변수와 프로퍼티의 이름을 지정할 때는 옵셔널이 이미 타입 선언에 있으므로 'optionalString` 또는`maybeView`와 같은 이름을 사용하지 마십시오.

옵셔널 바인딩의 경우, 'unwrappedView` 또는`actualLabel`과 같은 이름을 사용하지 말고 적절한 경우 원래 이름을 사용하십시오.

**Preferred:**
```swift
var subview: UIView?
var volume: Double?

// later on...
if let subview = subview, let volume = volume {
  // do something with unwrapped subview and volume
}
```

**Not Preferred:**
```swift
var optionalSubview: UIView?
var volume: Double?

if let unwrappedSubview = optionalSubview {
  if let realVolume = volume {
    // do something with unwrappedSubview and realVolume
  }
}
```

### Lazy Initialization

객체수명에대한 보다 미세한 제어를 위해 지연 초기화를 사용해보십시오. 이것은 뷰를 느리게 로드하는`UIViewController`에서 특히 그렇습니다. `{ } ()`라고하는 closure를 사용하거나 private factory 메소드를 호출 할 수 있습니다. 예:

```swift
lazy var locationManager: CLLocationManager = self.makeLocationManager()

private func makeLocationManager() -> CLLocationManager {
  let manager = CLLocationManager()
  manager.desiredAccuracy = kCLLocationAccuracyBest
  manager.delegate = self
  manager.requestAlwaysAuthorization()
  return manager
}
```

**Notes:**
  - `[unowned self]`은 여기에 필요하지 않습니다. 리테인 사이클이 발생하지 않습니다.
  - Location manager는 사용자에게 권한을 요청하기 위해 UI를 팝업하기위한 부작용이 있으므로 lazy가 적합합니다.

### Type Inference

컴파일러에서 단일 인스턴스의 상수 또는 변수 유형을 추론하게하십시오. 타입 추론은 작은 (비어 있지 않은) 배열과 딕셔너리에도 적합합니다. 필요한 경우 'CGFloat'또는 'Int16'과 같은 특정 유형을 지정하십시오.

**Preferred:**
```swift
let message = "Click the button"
let currentBounds = computeViewBounds()
var names = ["Mic", "Sam", "Christine"]
let maximumWidth: CGFloat = 106.5
```

**Not Preferred:**
```swift
let message: String = "Click the button"
let currentBounds: CGRect = computeViewBounds()
let names = [String]()
```

#### Type Annotation for Empty Arrays and Dictionaries

빈 배열 및 딕셔너리의 경우 타입 어노테이션을 사용하십시오.

**Preferred:**
```swift
var names: [String] = []
var lookup: [String: Int] = [:]
```

**Not Preferred:**
```swift
var names = [String]()
var lookup = [String: Int]()
```

### Syntactic Sugar

전체 제네릭 구문에 대한 타입 선언의 단축된 버전을 사용하십시오.

**Preferred:**
```swift
var deviceModels: [String]
var employees: [Int: String]
var faxNumber: Int?
```

**Not Preferred:**
```swift
var deviceModels: Array<String>
var employees: Dictionary<Int, String>
var faxNumber: Optional<Int>
```

## Functions vs Methods

클래스 또는 타입에 소속되어있지 않은 Free functions는 자제해야합니다. 이는 가독성과 발견 가능성을 도와줍니다.

Free functions는 특정 타입이나 인스턴스와 관련이 없을 때 가장 적합합니다.

**Preferred**
```swift
let sorted = items.mergeSorted()  // easily discoverable
rocket.launch()  // acts on the model
```

**Not Preferred**
```swift
let sorted = mergeSort(items)  // hard to discover
launch(&rocket)
```

**Free Function Exceptions**
```swift
let tuples = zip(a, b)  // feels natural as a free function (symmetry)
let value = max(x, y, z)  // another free function that feels natural
```

## Memory Management

비 프로덕션, 튜토리얼 데모 코드 조차도 리테인 사이클을 생성해서는 안됩니다. 'weak'및 'unowned' 참조로 오브젝트 그래프를 분석하고 강한 사이클을 방지하십시오. 또는 값 타입 (`struct`,`enum`)을 사용하여 순환을 방지하십시오.

### Extending object lifetime

`[weak self]`와`guard let strongSelf = self else { return }` 을 사용하여 객체 수명을 연장하십시오. `[weak self]`는 `[unowned self]`보다 선호되는데, `self`가 클로저보다 더 오래 지속된다는 것이 확실하지 않습니다. 옵셔널 언래핑보다는 명시적으로 수명을 연장하는 것이 좋습니다.

**Preferred**
```swift
resource.request().onComplete { [weak self] response in
  guard let strongSelf = self else {
    return
  }
  let model = strongSelf.updateModel(response)
  strongSelf.updateUI(model)
}
```

**Not Preferred**
```swift
// might crash if self is released before response returns
resource.request().onComplete { [unowned self] response in
  let model = self.updateModel(response)
  self.updateUI(model)
}
```

**Not Preferred**
```swift
// deallocate could happen between updating the model and updating UI
resource.request().onComplete { [weak self] response in
  let model = self?.updateModel(response)
  self?.updateUI(model)
}
```

## Access Control

튜토리얼의 전체 접근 제어자는 주요 주제에서 벗어날 수 있으며 반드시 필요하지는 않습니다. 그러나 `private`와 `fileprivate`를 적절하게 사용하면 명확성이 추가되고 캡슐화가 촉진됩니다. 가능하다면 `private`을 `fileprivate`로 선호하십시오. 확장 기능을 사용하려면 `fileprivate`를 사용해야합니다. // (swift 4에서는 private로 통일)

전체 접근 제어자 명세화가 필요할 때만, 명시적으로 `open`,`public` 및`internal` 만 사용하십시오.

주요 속성 지정자로 접근 제어자를 사용하십시오. 접근 제어 이전에 올 수 있는 것들은 `static` 지정자 또는 `@IBAction`, `@IBOutlet` 및 `@discardableResult`와 같은 속성입니다.

Use access control as the leading property specifier. The only things that should come before access control are the `static` specifier or attributes such as `@IBAction`, `@IBOutlet` and `@discardableResult`.

**Preferred:**
```swift
private let message = "Great Scott!"

class TimeMachine {  
  fileprivate dynamic lazy var fluxCapacitor = FluxCapacitor()
}
```

**Not Preferred:**
```swift
fileprivate let message = "Great Scott!"

class TimeMachine {  
  lazy dynamic fileprivate var fluxCapacitor = FluxCapacitor()
}
```

## Control Flow

`while-condition-increment` 스타일보다 `for-for` 스타일의 for 루프를 선호하십시오.

**Preferred:**
```swift
for _ in 0..<3 {
  print("Hello three times")
}

for (index, person) in attendeeList.enumerated() {
  print("\(person) is at position #\(index)")
}

for index in stride(from: 0, to: items.count, by: 2) {
  print(index)
}

for index in (0...3).reversed() {
  print(index)
}
```

**Not Preferred:**
```swift
var i = 0
while i < 3 {
  print("Hello three times")
  i += 1
}


var i = 0
while i < attendeeList.count {
  let person = attendeeList[i]
  print("\(person) is at position #\(i)")
  i += 1
}
```

## Golden Path

조건문을 사용하여 코딩 할 때 코드의 왼쪽 여백은 "golden"또는 "happy" 경로여야합니다. 즉, `if`문을 중첩하지 마십시오. 여러 개의 return 문은 OK 입니다. `guard` 문은 이것을 위해 만들어졌습니다.

**Preferred:**
```swift
func computeFFT(context: Context?, inputData: InputData?) throws -> Frequencies {

  guard let context = context else {
    throw FFTError.noContext
  }
  guard let inputData = inputData else {
    throw FFTError.noInputData
  }

  // use context and input to compute the frequencies
  return frequencies
}
```

**Not Preferred:**
```swift
func computeFFT(context: Context?, inputData: InputData?) throws -> Frequencies {

  if let context = context {
    if let inputData = inputData {
      // use context and input to compute the frequencies

      return frequencies
    } else {
      throw FFTError.noInputData
    }
  } else {
    throw FFTError.noContext
  }
}
```

여러 옵션이 `guard` 또는 `if let`으로 풀려있는 경우 가능한 경우 복합 버전을 사용하여 중첩을 최소화하십시오. 예:

**Preferred:**
```swift
guard let number1 = number1,
      let number2 = number2,
      let number3 = number3 else {
  fatalError("impossible")
}
// do something with numbers
```

**Not Preferred:**
```swift
if let number1 = number1 {
  if let number2 = number2 {
    if let number3 = number3 {
      // do something with numbers
    } else {
      fatalError("impossible")
    }
  } else {
    fatalError("impossible")
  }
} else {
  fatalError("impossible")
}
```

### Failing Guards

Guard 문은 어떤 식 으로든 종료해야합니다. 일반적으로 이것은 `return`, `throw`, `break`, `continue`, `fatalError ()`와 같은 간단한 한 줄의 문장이어야합니다. 큰 코드 블록은 피해야합니다. 여러 종료 점에 정리 코드가 필요한 경우, 정리 코드 중복을 피하기 위해 `defer` 블록을 사용하는 것을 고려하십시오.

## Semicolons

Swift는 코드의 각 문 다음에 세미콜론이 필요하지 않습니다. 한 행에 여러 명령문을 결합하려는 경우에만 필요합니다.

**Preferred:**
```swift
let swift = "not a scripting language"
```

**Not Preferred:**
```swift
let swift = "not a scripting language";
```
## Parentheses

조건문 주위의 괄호는 필요하지 않으므로 생략해야합니다.

**Preferred:**
```swift
if name == "Hello" {
  print("World")
}
```

**Not Preferred:**
```swift
if (name == "Hello") {
  print("World")
}
```

더 큰 표현식에서는 선택적인 괄호로 인해 코드를 더 명확하게 읽을 수 있습니다.

**Preferred:**
```swift
let playerMark = (player == current ? "X" : "O")
```
