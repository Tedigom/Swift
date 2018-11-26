# Apple Face Detection with
## 1. Core Image / 2. Vision

---
애플은 CIDetector 클래스를 통해 코어 이미지 프레임워크의 공개 API에서 얼굴 탐지 기능을 처음 공개하였습니다. 이 API는 포토와 같은 Apple 앱에서도 내부적으로 사용되었습니다.

CIDetector의 초기 버전은 Viola-Jones 탐지 알고리즘에 기반한 방법을 사용했습니다. 이후 CIDetector의 개선은 전통적인 컴퓨터 비전의 발전에 기초하였습니다.

그러나 딥러닝의 출현과 컴퓨터 vision 문제에 대한 딥러닝 적용으로, 얼굴 감지 정확성의 최첨단 기술이 큰 도약을 했습니다.

그리고 이를 적절히 활용하여 2017년, CoreML과 Vision이 등장하였습니다.

[Viola-Jones 탐지 알고리즘](https://en.wikipedia.org/wiki/Viola–Jones_object_detection_framework)

[애플의 Face Detection 진보와 Vision 원리 설명](https://machinelearning.apple.com/2017/11/16/face-detection.html#1)

---

## 1. Core Image

Core Image는 정지된 영상과 비디오 영상을 위해 실시간에 가까운 처리 기능을 제공하도록 설계된 영상 처리 및 분석 기술입니다. GPU 또는 CPU 렌더링 경로를 사용하여 Core Graphics, Core Video 및 Image I/O 프레임워크의 이미지 데이터 유형에서 작동합니다.

### Face Detection using Core Image ###

A: Convert the UIImage to CIImage

```swift
let ciimage = CIImage(image: UIImage)
```

B: Set the CIDetector accuracy
```swift
let detectOption = [CIDetectorAccuracy: CIDetectorAccuracyHigh]
```

C: Create an object of CIDetector of typeface with options (CIDetector는 정지된 이미지 또는 비디오에서 중요한 특징(예: 얼굴 및 바코드)을 식별하는 이미지 프로세서입니다.)
```swift
 let faceDetector = CIDetector(ofType: CIDetectorTypeFace, context: nil, options: detectOption)
```
D: Call features function of CIDetector which will extract and return an array of faces from the given image.
 ```swift
 let faces = faceDetector.features(in: ciimage)
 ```

#### Observation Result ####

 <img src='./assets/ci.png' width='450'>

 ---


## 2. Vision

Vision은 머신러닝을 이용하여 고성능 이미지 분석 및 얼굴 식별, 영상 및 비디오 장면 분석을 위한 컴퓨터 비전 기법을 적용합니다.

### Face Detection using Vision ###

A: 얼굴 검출을 요청하는 이미지 분석 리퀘스트인 VNDetectFaceRectanglesRequest와 VNSequenceRequestHandler를 initialize 합니다.

```swift
let faces = faceDetector.features(in: ciimage)
```
B: Convert the UIImage to CIImage

```swift
let faceDetectionRequest = VNDetectFaceRectanglesRequest()

let faceDetectionHandler = VNSequenceRequestHandler()
````
C: Perform the face detection request on the given image

```swift
try? faceDetectionHandler.perform([faceDetectionRequest], on: ciimage)
````
D: Return an array of Faces

```swift
let results = faceDetection.results as? [VNFaceObservation]
````

#### Observation Result ####

 <img src='./assets/vn.png' alt='async_problem.png' width='450'>



---

# Firebase MLKit를 이용한 image Labeling

ML Kit의 얼굴 감지 API를 사용하면 영상에서 얼굴을 감지하고 주요 얼굴 특징을 식별할 수 있습니다.

얼굴 탐지 기능을 사용하면 자아와 초상화를 윤색하거나 사용자 사진에서 아바타를 생성하는 것과 같은 작업을 수행하는 데 필요한 정보를 얻을 수 있습니다.

UIImage 혹은 CMSampleBufferRef를 이용하여 요청하면 해당 사진에 대한 얼굴 정보가 비동기적으로 받아와집니다.

애플의 vision등과 비교하였을 때,

Apple vision은 검출된 face에 대해서 tracking 하지 않기 때문에 실시간 검출의 결과가 일정하지 않지만 GPU를 이용하여 매우 빠릅니다.

Google firebase face detection은 검출된 face에 대해서 tracking 하기 때문에 균일하게 이어지는 실시간 검출에 대해서 유리하지만, CPU만을 이용하기 때문에 상대적으로 느립니다.

자세한 사용법은 [Firebase 사용법](
https://firebase.google.com/docs/ml-kit/ios/detect-faces) 을 참조할 수 있습니다.
