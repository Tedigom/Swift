# LLVM & Clang

Swift의 3가지 큰 특징 중 하나인 fast를 위한 애플의 노력 결과입니다.


## LLVM

 우리는 앱을 개발하기 위해 소스코드를 작성합니다. 그리고 나서 작성한 소스코드를 빌드한 후 실행합니다. 소스코드는 사람이 이해할 수 있는 형태의 언어로 작성된 코드입니다. 기계(iPhone, iPad, Macbook 등)가 이 소스코드를 실행하려면 소스코드를 기계가 이해할 수 있는 형태 즉, 바이너리나 기계어로 변환해야 합니다. 이 변환을 하는 것이 컴파일러의 역할입니다.

 컴파일러는 보통 Frontend, Optimzer, Backend의 세 가지 구성요소를 가지고 있습니다.

<img src = "http://farm1.staticflickr.com/763/22223263753_68d8d34c7e_o.png">

- Frontend : 어휘분석(Lexical analysis), 구문분석(Syntax analysis), 의미분석(Semantic analysis), 중간코드 생성(Immediate code generation)


- Optimizer : 런타임시 성능향상을 위해 중복계산 제거 및 기타 여러 변환 실행

- Backend : 각 코드를 타켓 아키텍처에 맞는 인스트럭션 셋(Instruction Set)으로 매핑해 실행코드 생성

LLVM은 이러한 기존 컴파일러의 비효율성을 개선하고자 나온 컴파일러 입니다. LLVM은 기존의 기존의 컴파일러의 3가지 주요 구성요소를 분리해서 각각의 부분들을 조합할 수 있게 디자인 했습니다.

<img src = "http://farm1.staticflickr.com/780/22844403335_edcf4a9f53_o.png">

소스 코드는 먼저 Swift Abstract Syntax Tree (AST)로 변환됩니다. 다음 Swift Intermediate Language(SIL)로 변환되어 Swift를 위한 최적화를 실시한 후 LLVM IR로 변환 된 LLVM Optimizer에 보내집니다.

LLVM이 기존의 컴파일러의 3가지 구성요소를 분리했다는 것 외에 가장 다른 점은 바로 비트코드라 불리는 중간코드(IR : Intermediate Representation)를 생성한다는 것입니다. 그래서 Frontend에서는 비트코드를 생성하고 Backend에서는 이 코드를 받아서 타겟에 맞는 결과물을 생성합니다. 이 비트코드를 사용함으로 얻는 장점은 효율성/확장성입니다.

만약 일반적인 컴파일러가 새로운 언어를 지원한다고 가정해 보고 그 언어를 K라고 해보겠습니다. K언어를 x86아키텍처, PowerPC, ARM등 각기 다른 아키텍처의 기계어를 생성하려면 3개의 컴파일러를 작성해야 하고 컴파일러 각각의 Frontend, Optimiser, Backend를 구현해 주어야 합니다. 하지만 LLVM을 이용하면 K언어의 Frontend 부분만 개발하면 여러 아키텍처를 타겟으로 빌드할 수 있습니다. (clang이 LLVM에서 C, C++, Objective-C같은 C언어 계열의 Frontend입니다.)

LLVM 덕분에 GCC(GNU Compiler Collection)와 비교했을때, 컴파일 속도는 2배 빨라지고, 빌드된 실행파일의 크기도 2~30% 줄일 수 있었습니다. 실행 속도 또한 파일의 크기가 줄어든 것과 비례에 빨라졌습니다. 뿐만아니라 LLVM가 있었기때문에 Block을 지원하고 GCD(Grand Central Dispatch)가 가능해졌습니다. Xcode의 다양한 Instruments 기능과 Xcode IDE와의 연동도 역시 LLVM 덕분입니다.

## Clang

LLVM은 컴파일과 링크 시 런타임 등 모든 지점에서 프로그램을 최적화 할 수 있는 컴파일러 기반이다. 현재 최신 버전 Xcode 컴파일러 프론트엔드에 Clang, 백엔드에 LLVM 조합이 사용되고 있지만, LLVM이 채택 된 당초는 프론트엔드는 GCC가 사용 되고 있었습니다. 기존 Objective-C 컴파일러는 GCC를 이용하고 있었지만, 확장이 어렵기 때문에 Apple은 Clang을 개발했습니다. 여러 장점이 있겠지만 대표적인 Clang의 장점으로 메모리 관리의 참조 카운트 방식을 말할 수 있습니다. 기존 GCC의 경우 가비지 컬렉션을 통해 메모리 관리를 시도합니다. 대표적으로 GC의 부작용이 2가지 언급되어지곤 합니다.

-  1. GC가 언제 실행될 지 코드 상에서 알 수가 없습니다.

-  2. GC로 인한 퍼포먼스 저하  

그러나 ARC(Automatic Reference Counting)와 Static Analyzer의 등장으로 메모리 관리가 쉬워졌으며 퍼포먼스도 좋아졌습니다. ARC는 컴파일 시에 참조 횟수를 증감하는 코드를 자동으로 채웁니다. Static Analyzer를 사용하면 정적으로 잠재적인 메모리 누수를 자동으로 감지 할 수 있습니다. 이러한 Clang을 통한 확장으로 Swift의 성능이 향상되고 있다고 말할 수 있습니다.  

### Reference:

http://kyejusung.com/2015/11/llvm%EC%9D%B4%EB%9E%80-clang-%EB%B9%84%ED%8A%B8%EC%BD%94%EB%93%9C-%ED%8F%AC%ED%95%A8/
