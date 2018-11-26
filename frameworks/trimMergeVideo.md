# Trim & Merge Video

## Trim & Merge 정리

Media에 대해 원본의 내용을 변환하여 사전에 지정된 Export 지정 형식으로 출력물을 뽑아내기 위해서는 AVAssetExportSession을 사용하여 비동기적으로 export를 진행하여야 합니다. 단순히 하나의 asset을 가지고 trim하는 등의 변형이 필요한 경우에는 이러한 AVAssetExportSession과 원본 asset을 통해 time range를 조정함으로써 해당 작업을 수행할 수 있습니다.

그러나 여러 개의 asset을 이용하여 새로운 composition을 생성해야 하는 경우, AVComposition을 이용하여야 합니다. 새로운 Composition을 생성한 후, 변형하고자 하는 asset의 정보를 기반으로 하여 audio track, video track 등의 Composition 내의 미디어 트랙과 Composition을 통해 수행될 작업인 Instruction을 지정함으로서 Merge등의 작업을 수행할 수 있습니다.

이러한 Compostion에서 AVVideoCompositionCoreAnimationTool등을 추가로 이용하면 비디오 위에 특정 layer를 add하여 export할 수도 있습니다.

## AVAssetExportSession

파라미터로 받은 Asset 원본 객체의 내용을 변환하여 사전에 지정된 Export 지정 형식의 출력물 객체를 생성하는 Session 객체입니다.

presetName과 outputFileType 의 정보와 함께 세션을 초기화 한 후, exportAsynchronously(completionHandler:)를 비동기식으로 호출하여 실행할 수 있습니다. 내보내기가 비동기식으로 수행되므로 이 함수  즉시 리턴(반환)됩니다. progress를 확인하여 진행 상황을 확인할 수도 있습니다. multiple exports가 실행될 일부 export가 대기열에 포함될 수도 있습니다. (이 때의 session status는 AVAssetExportSession.Status.waiting입니다.)

cf) 이미 존재하는 파일이 존재하는 등의 오류가 있다면 AVAssetExportSession.Status.failure의 상태를 리턴합니다.

(completionHandler:)는 내보내기가 실패, 완료 또는 취소되었는지 여부를 나타냅니다. 완료 시 상태 속성은 내보내기가 성공적으로 완료되었는지 여부를 나타냅니다. 오류가 발생한 경우 오류 속성 값은 실패 원인에 대한 추가 정보를 제공합니다.

outputURL을 통해 export session의 output의 url을 명시적으로 지정할 수 있습니다.

```swift
guard let exportSession = AVAssetExportSession(asset: asset, presetName: AVAssetExportPresetHighestQuality) else { return }
        exportSession.outputURL = url
        exportSession.outputFileType = .mp4
        exportSession.shouldOptimizeForNetworkUse = true
        let timeRange = CMTimeRange(start: CMTime(seconds: startTime, preferredTimescale: asset.duration.timescale),
                                    end: CMTime(seconds: endTime, preferredTimescale: asset.duration.timescale))

        exportSession.timeRange = timeRange

        exportSession.exportAsynchronously {
            DispatchQueue.main.async {
                self.exportDidFinish(exportSession) { outputURL in
                    completion(outputURL)
                }
            }
        }

func exportDidFinish(_ session: AVAssetExportSession, completion: @escaping ((_ outputUrl: URL) -> Void)) { }
```

위에서 처럼 exportSession의 timeRange를 startTime과 endTime을 지정함으로써 비디오를 trim 할 수 있습니다. 이외에도 outputFileType등 output을 configuring하기 위한 다수의 property가 존재합니다.

Merge는 아래의 추가적인 작업을 필요로 합니다.  

## AVMutableComposition

기존 존재하는 asset에서 새로운 구성을 생성하는 데에 사용되는 mutable (변형 가능) 객체입니다.

이 클래스는 트랙을 추가 및 제거하는 기능을 제공하며, 시간 범위를 추가, 제거 및 확장할 수 있습니다. playback, inspection등을 위해서 mutable composition의 immutable (변형 불가능한) 스냅샷을 만들수도 있습니다.

이러한 composition내에서 track, instruction 등을 설정하여 여러 개의 비디오를 Merge하는 등의 변형을 가할 수 있습니다.

```swift
// Init composition
let mixComposition = AVMutableComposition.init()

// Main video composition instruction
let mainInstruction = AVMutableVideoCompositionInstruction()
mainInstruction.timeRange = CMTimeRangeMake(kCMTimeZero, insertTime)
mainInstruction.layerInstructions = arrayLayerInstructions

// Main video composition
let mainComposition = AVMutableVideoComposition()
mainComposition.instructions = [mainInstruction]
mainComposition.frameDuration = CMTimeMake(1, 30)
mainComposition.renderSize = outputSize

// Create Exporter
guard let exporter = AVAssetExportSession(asset: mixComposition, presetName: AVAssetExportPresetHighestQuality) else { return }
exporter.outputURL = url
exporter.outputFileType = AVFileType.mp4
exporter.shouldOptimizeForNetworkUse = true
exporter.videoComposition = mainComposition

// Perform the Export
exporter.exportAsynchronously() {
    DispatchQueue.main.async {
        self.exportDidFinish(exporter) { url in
            completion(url)
        }
    }
}
```      

## AVCompositionTrack

미디어 유형, 트랙 식별자 및 트랙 세그먼트로 구성된 Composition 객체 내의 트랙입니다.

```swift
// Init video & audio composition track
let videoCompositionTrack = mixComposition.addMutableTrack(withMediaType: AVMediaType.video,
                                                          preferredTrackID: Int32(kCMPersistentTrackID_Invalid))

let audioCompositionTrack = mixComposition.addMutableTrack(withMediaType: AVMediaType.audio,
                                                          preferredTrackID: Int32(kCMPersistentTrackID_Invalid))

do {
   let startTime = kCMTimeZero
   let duration = videoAsset.duration

   // Add video track to video composition at specific time
   try videoCompositionTrack?.insertTimeRange(CMTimeRangeMake(startTime, duration),
                                              of: videoTrack,
                                              at: insertTime)

   // Add audio track to audio composition at specific time
   if let audioTrack = audioTrack {
       try audioCompositionTrack?.insertTimeRange(CMTimeRangeMake(startTime, duration),
                                                  of: audioTrack,
                                                  at: insertTime)
   }

   // Add instruction for video track
   let layerInstruction = VideoHelper.videoCompositionInstruction(for: videoCompositionTrack!,
                                                                  asset: videoAsset,
                                                                  standardSize: outputSize,
                                                                  atTime: insertTime)

   }

catch {
        print("Load track error")
    }
```

## AVCompositionInstruction

AVCompositionInstruction은 컴포지션에 의해 수행될 작업을 나타내며, timeRange 속성을 asset이 재생될 시간 범위로 설정해야 합니다. 여기서는 비디오가 시작되는 시간과 비디오가 지속되는 시간을 지정해야 합니다.

AVVideoComposition은 합성 비디오 프레임을 만드는 데 사용될 비디오 트랙의 수와 ID를 설명합니다. frameDuration을 30 fps로 설정하고, 렌더 크기를 생성할 크기로 설정합니다.

```swift
let mainInstruction = AVMutableVideoCompositionInstruction()
mainInstruction.timeRange = CMTimeRangeMake(kCMTimeZero, insertTime)
mainInstruction.layerInstructions = arrayLayerInstructions

// Main video composition
let mainComposition = AVMutableVideoComposition()
mainComposition.instructions = [mainInstruction]
mainComposition.frameDuration = CMTimeMake(1, 30)
```
