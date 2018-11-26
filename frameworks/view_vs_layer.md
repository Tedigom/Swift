# View vs layer

모든 화면은 뷰(View) 단위로 구성됩니다. 최상위 윈도우가 있더라도 모든 화면은 뷰가 쌓인 구조로 표시됩니다.

하나의 뷰는 뷰 그리기 루틴(drawRect), 서브뷰(Subviews)들과 함께 레이어(Layer)라는 부품들로 구성됩니다. 레이어는 뷰에서 눈에 보여지는 부분을 다루는 중요한 기능입니다.

뷰에는 하나의 레이어만 있을 수 있습니다. 대신 레이어는 서브레이어(Sublayers)를 여럿 가질 수 있습니다. 그리고 이름답게 다층(multi layer) 구조로 구성 할 수 있습니다.

뷰를 여러개 쌓아서 만드는 것에 비해, 같은 모양을 레이어를 쌓아서 만드는게 훨신 가볍기 때문에 애니메이션 등에 유리합니다.

간단히 말해서, CALayer는 NSObject에서 상속되며, **렌더링, 애니메이션 등의 보이는 부분에 초점을 맞춥니다.**  layer의 기본 작업은 사용자가 제공하는 시각적 콘텐츠를 관리하는 것이지만 배경색, 테두리 및 그림자 같은 시각적 속성 역시 설정되어 있습니다. 시각적 콘텐츠 관리 외에도, 계층은 해당 콘텐츠를 화면에 표시하는 데 사용되는 콘텐츠의 형태(위치, 크기 및 변환 등)에 대한 정보도 유지합니다.


UIView는 **이러한 Layer를 포함하는 Container이며 UIResponder에서 상속되어 사용자의 이벤트 등 역시 처리합니다.**

정리하여 나타내자면 다음과 같습니다.

**UIView** : Container

뷰가 어떻게 표현되어야 할지 Layer에 delegate 및 User Interaction 핸들

- layer(CALayer)를 속성으로 가지고 있고 이것을 관리할 의무가 있음

- 추가적으로 User interatcion 을 처리함

**CALayer** : 보여지는 것 담당

애플 프레임웍에서 visual content를 어떻게 구성할것인가를 전적으로 담당(drawing, layout, animation)

- CALayer 만 할수 있는것 (UIView 가 못하는것)

  - Drop shadows, rounded corner, colored border

  - 3D transforms and positioning

  - Nonrectangular bounds

  - Alpha masking of content

  - Multistep, nonlinear animations
