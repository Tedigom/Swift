# Save Video

## PHAssetChangeRequest

사진 라이브러리 변경 블록에 사용하기 위해 사진 asset의 내용을 작성, 삭제, 변경 또는 편집하기 위한 요청입니다.

1. 작성

3가지의 방법이 존재합니다.

- 1. class func creationRequestForAsset(from: UIImage) -> Self

- 2. class func creationRequestForAssetFromImage(atFileURL: URL) -> Self?

- 3. class func creationRequestForAssetFromVideo(atFileURL: URL) -> Self?

  - var placeholderForCreatedAsset: PHObjectPlaceholder?

2. 삭제

- deleteAssets(_ : ) 함수 호출하여 asset을 삭제합니다.

3. 변경

- init(for : ) 함수 호출하여 asset content 및 메타데이터를 수정합니다.  

cf) isFavorite property 설정을 통해 favorite asset으로 마킹할 수 있습니다.

```swift
let saveVideoToPhotos = {
    PHPhotoLibrary.shared().performChanges({
                      let assetRequest = PHAssetChangeRequest.creationRequestForAssetFromVideo(atFileURL: outputURL)
                      assetPlaceholder = assetRequest?.placeholderForCreatedAsset
                  }, completionHandler: { (success, error) in

                    // code

    })
}

// Ensure permission to access Photo Library
if PHPhotoLibrary.authorizationStatus() != .authorized {
    PHPhotoLibrary.requestAuthorization({ status in
        if status == .authorized {
            saveVideoToPhotos()
        } else {
            print(status.rawValue)
        }
    })
} else {
    saveVideoToPhotos()
}

```

### Create assets

#### 1.  class func creationRequestForAsset(from: UIImage) -> Self

사진 라이브러리에 새 이미지 asset 추가 요청을 만듭니다.

#### 2.  class func creationRequestForAssetFromImage(atFileURL: URL) -> Self?

지정된 URL의 이미지 파일을 사용하여 사진 라이브러리에 새 이미지 asset 추가 요청을 만듭니다.

#### 3.  class func creationRequestForAssetFromVideo(atFileURL: URL) -> Self?

지정된 URL의 비디오 파일을 사용하여 사진 라이브러리에 새 비디오 asset 추가 요청을 만듭니다.

#### var placeholderForCreatedAsset: PHObjectPlaceholder?

변경 요청을 통해 생성하는 asset에 대한 placeholder 객체입니다. 동일한 변경 블록 내에서 변경 요청으로 생성된 asset을 참조해야 하는 경우 이 속성을 사용할 수 있습니다. 예를 들어 아래의 경우 처럼, asset을 생성 후 해당 asset url을 completion의 인자로 넘겨줄 수 있습니다.

```swift

PHPhotoLibrary.shared().performChanges({
        let assetRequest = PHAssetChangeRequest.creationRequestForAssetFromVideo(atFileURL: outputURL)
        let assetPlaceholder = assetRequest?.placeholderForCreatedAsset
    }, completionHandler: { (success, error) in
        if success {
            let localID = assetPlaceholder.localIdentifier
            let assetID = localID.replacingOccurrences(of: "/.*", with: "", options: NSString.CompareOptions.regularExpression, range: nil)
            let ext = "mp4"
            let assetURLStr = "assets-library://asset/asset.\(ext)?id=\(assetID)&ext=\(ext)"

            if let url = URL(string: assetURLStr) {
                print(url)
                completion(url)
            } else {
                print("something wrong")
            }

        } else {
            print(error.debugDescription)
        }
    })
```
