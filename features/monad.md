# Monad

Monad는 값이 있을 수도 있고 없을 수도 있는 상태를 포장하는 타입입니다. (Swift에서 Optional이 이에 해당합니다.) Monad는 [Functor](./functor.md) 의 한 유형입니다.

Functor와 Monad는 컨테이너 타입에 적용할 수 있는 공통 연산을 기술하기 위한 하나의 디자인 패턴이라고 볼 수 있습니다.

이러한 Monad는 중첩되어 wrapping된 결과물을 flat하여주는 flatMap과 함께 사용할 때 매우 효과적입니다. ~~(Optional(Optional(Optional(5)))와 같은 이상한 타입을 즐기는 취향이 아닌 이상...)~~ flatMap과 함께 사용할 때의 대표적인 예를 그림으로 그려 표현하면 아래와 같습니다.

<img src="./assets/monad.png">

 ### Reference:
 - http://hiddenviewer.tistory.com/275
