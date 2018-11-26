

# Buffer for Data processing

### CMSampleBuffer
CMSampleBuffer는 미디어 샘플 데이터를 파이프라인을 통해 이동하는 데 사용되는 **특정 미디어 유형(오디오, 비디오, muxed 등)의 0개 이상의 압축(또는 압축되지 않은) 샘플을 포함하는 핵심 기반 개체**입니다. 샘플 버퍼에는 샘플 레벨 및 버퍼 레벨 첨부 파일이 모두 포함될 수 있습니다. 샘플 레벨 첨부 파일은 버퍼의 각 개별 샘플(프레임)과 연결되며 타임스탬프 및 비디오 프레임 종속성과 같은 정보를 포함합니다.

- CMBlockBuffer:

하나 이상의 미디어 샘플

- CVImageBuffer:

각 미디어 샘플의 크기 및 타이밍 정보, 버퍼 레블 과 샘플 레벨의 첨부 파일, 다양한 유형의 이미지 데이터를 관리하기 위한 인터페이스입니다.

 CMSampleBufferGetSampleAttachmentsArray(_:createIfNecessary:)  기능을 사용하여 샘플 레벨 첨부 파일을 읽고 쓸 수 있습니다. 버퍼 수준 첨부 파일은 재생 속도 및 버퍼 사용 시 수행할 작업과 같이 버퍼 전체에 대한 정보를 제공합니다.


### CVBuffer

CVBuffer는 **데이터의 버퍼와 상호 작용하는 방법을 정의하는 추상 기본 클래스 역할**을 합니다. 버퍼 개체는 비디오, 오디오 또는 다른 유형의 데이터를 저장할 수 있습니다.

CVImageBuffer 및 CVPixelBuffer와 같이 Core Video에 의해 정의된 다른 모든 버퍼 유형은 CVBuffer에서 파생됩니다. 모든 코어 비디오 버퍼에서 CVBuffer 프로그래밍 인터페이스를 사용할 수 있습니다.

### CVImageBuffer

CVImageBuffer는 **다양한 유형의 이미지 데이터를 관리하기 위한 인터페이스**입니다. 픽셀 버퍼 및 Core Video OpenGL 버퍼는 Core Video 이미지 버퍼에서 파생됩니다.

### CVPixelBuffer

CVPixelBuffer는 **메인 메모리에 픽셀을 저장하는 이미지 버퍼입**니다. 프레임 생성, 비디오 압축 또는 압축 풀기 또는 Core Image를 사용하는 애플리케이션은 모두 Core Video 픽셀 버퍼를 사용할 수 있습니다.
