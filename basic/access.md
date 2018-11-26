# Access Control (접근 제어자)

Swift의 접근 제어자(Access Control)에 대해서 간단히 정리해보고자 합니다. 정의를 살펴보면, 다음과 같습니다.

> Access control restricts access to parts of your code from code in other source files and modules.

접근 제어자는 코드를 작성하는 한 파일에서 다른 파일에 있는 **코드에 대한 접근을 명시적으로 작성**하여 이를 관리하는 것인데, module과 source file에 따라 다른 접근을 할 수 있습니다.

## Access Control 사용 이유

에플리케이션이 커진다는 것은 다른 말로 망가질 확률이 커진다는 의미와 같습니다. 특히 로직이 망가지는 첫번째 용의자는 사용자입니다. 즉 객체를 사용하는 입장에서 객체 내부적으로 사용하는 변수나 메소드에 접근함으로서 개발자가 의도하지 못한 오동작을 일으키게 되는 것입니다.

이런 문제로부터 객체의 로직을 보호하기 위해서는 맴버에 따라서 외부의 접근을 허용하거나 차단해야 할 필요가 생깁니다. 마치 은행이 누구나 접근 할 수 있는 창구와 관계자외에는 출입이 엄격하게 통제되는 금고를 구분하고 있는 이유와 같습니다.

<img src="https://s3.ap-northeast-2.amazonaws.com/opentutorials-user-file/module/516/1979.jpg">


접근 제어자를 사용하는 또 다른 이유는 사용자에게 객체를 조작 할 수 있는 수단만을 제공함으로서 결과적으로 객체의 사용에 집중 할 수 있도록 돕기 위함입니다. 어떤 맴버에 대한 접근을 허용할 것인가를 작업자의 판단에 달렸습니다.

## Module과 Source file

module이라는 것은 하나의 프레임워크를 의미합니다. 즉, import 키워드로 추가되는 것들이 module입니다. UIKit, Foundation 등이 모두 module입니다. 프로젝트의 하위에 있는 targets도 각각 모두 하나의 module입니다. source file은 각각의 module 안에 있는 파일들입니다. 예를 들어 example.swift 같은 파일들이 하나의 source file입니다.

## Swift의 5가지 접근 제어자

이제 module과 source file을 기준으로 나뉘는 5가지 접근 제어자를 알아보고자 합니다. 그리고 여기서는 특정 접근 제어자가 적용되는 대상을 entity로 서술합니다. entity는 접근제어자를 작성할 수 있는 property, method, class, struct 등의 집합을 의미합니다.

- 1. open, public - 프로젝트 내의 모든 module 해당 entity에 접근할 수 있습니다.

- 2. internal - default 접근 제어자로, entity가 작성된 module에서만 접근할 수 있습니다.

- 3. fileprivate - entity가 작성된 source file에서만 접근할 수 있도록 합니다. 서로 다른 클래스가 같은 파일안에 있고 fileprivate로 선언되어 있다면 둘은 서로 접근할 수 있습니다.

- 4. private - 특정 객체에서만 사용할 수 있도록 하는 가장 제한적인 접근제어자입니다. fileprivate과 달리 같은 파일 안에 있어도 서로 다른 객체가 private로 선언되어 있다면 둘은 서로 접근할 수 없습니다.

### open과 public의 차이

 둘의 차이는 open은 다른 모듈에서 subclass가 가능하지만, public은 그렇지 않다는 것입니다. 먼저 open은 class에만 사용될 수 있습니다. 그리고 한 모듈에서 만든 class를 superClass로 하는 subClass를 다른 모듈에서 만들기 위해서는 해당 superClass가 open으로 선언되어야 합니다. 당연히 overriding도 이 규칙이 적용됩니다.


### Reference:
- https://hcn1519.github.io/articles/2018-01/Swift_AccessControl
- https://opentutorials.org/module/516/6061
