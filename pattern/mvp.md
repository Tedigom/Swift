# MVP

<img src="./assets/mvp.png" width=600>


- 겉보기에는 MVC 패턴과 유사하지만 MVC 패턴은 View와 Controller가 팽팽히 묶여있지만 MVP 패턴에서는 Controller가 중재자 역할만을 수행함
- UIViewController를 View로써 다룸
- Presenter는 ViewController의 Life Cycle에 관여하지 않음
- Presenter는 쉽게 View를 Mocking 할 수 있음
- Presenter는 layout코드가 없으나, data를 가지고 View를 업데이트하는 책임을 가지고 있음
