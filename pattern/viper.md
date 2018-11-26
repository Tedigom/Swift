# VIPER

MV(X)에서 파생된건 아니지만 5개의 layer로 책임이 분배된 아키텍쳐(View, Interactor, Presenter, Entity, Router)

<img src="./assets/viper.png" width=600>

- View
  - UIViewController에 해당
  - Presenter에게 유저 액션을 알림
- Presenter
  - View와 Interactor의 중간 다리 역할
  - View에 대한 비즈니스 로직 처리
  - UIKit에 독립되어 있음
- Interactor
  - Data(Entity)에 대한 비즈니스 로직 처리
  - Service, Manager class (네트워크 처리 등등..)
- Entity
  - data Object
  - data access layer 아님(Interactor가 담당)
- Router
  - VIPER 모듈간 연결고리
