# Swift Lint Library

프로젝트가 커지다보면 협업하는 개발자가 많아지고, 각자 다른 코드 컨벤션 때문에 소스코드를 해석하는데 어려움이 생깁니다.

ex) 중괄호 위치

A 개발자
```swift
class A {
    ...
}
```
B 개발자
```swift
class B
{
    ...
}
```

이러한 스타일을 공통적으로 적용하기 위해 Swift Convention을 관리해주는 오픈소스로 SwiftLint가 존재합니다. SwiftLint는 Realm에서 개발한 스위프트 스타일과 컨벤션을 강제로 설정하는 툴입니다. 이를 통해 코드 스타일 및 컨벤션을 강제화해서 가독성이 좋고 일관된 코드를 사용할 수 있습니다.

- github: https://github.com/realm/SwiftLint
- 참고사이트: https://swifting.io/blog/2016/03/29/11-swiftlint/
