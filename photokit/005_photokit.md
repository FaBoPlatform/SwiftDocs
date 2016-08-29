# アルバムにアセットを追加する

![Preview PhotoKit005](./img/PhotoKit005.png) 

## Swift 3.0

`Info.plist`に`NSCameraUsageDescription`、`NSMicrophoneUsageDescription`、`NSPhotoLibraryUsageDescription`を追加します

```swift
//
//  ViewController.swift
//  phptokit005
//
//  Copyright © 2016年 FaBo, Inc. All rights reserved.
//
import UIKit
import AVFoundation
import Photos

class ViewController: UIViewController, AVCaptureFileOutputRecordingDelegate {
    
    // セッション.
    var mySession: AVCaptureSession!
    // デバイス.
    var myDevice: AVCaptureDevice!
    // 画像のアウトプット.
    var myImageOutput: AVCaptureStillImageOutput!
    // ビデオのアウトプット.
    var myVideoOutput: AVCaptureMovieFileOutput!
    // スタートボタン.
    var myButtonStart: UIButton!
    // ストップボタン.
    var myButtonStop: UIButton!
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        // セッションの作成.
        mySession = AVCaptureSession()
        // デバイス一覧の取得.
        let devices = AVCaptureDevice.devices()
        
        // バックライトをmyDeviceに格納.
        for device in devices! {
            if(device.position == AVCaptureDevicePosition.back){
                myDevice = device as! AVCaptureDevice
            }
        }
        
        // バックカメラを取得.
        let videoInput = try! AVCaptureDeviceInput.init(device: myDevice)
        
        // ビデオをセッションのInputに追加.
        mySession.addInput(videoInput)
        
        // マイクを取得.
        let audioCaptureDevice = AVCaptureDevice.devices(withMediaType: AVMediaTypeAudio)
        //let audioCaptureDevice = AVCaptureDevice.devicesWithMediaType(AVMediaTypeAudio)
        
        // マイクをセッションのInputに追加.
        let audioInput = try! AVCaptureDeviceInput.init(device: audioCaptureDevice?.first as! AVCaptureDevice)
        //let audioInput = try! AVCaptureDeviceInput.init(device: audioCaptureDevice.first as! AVCaptureDevice)
        
        // オーディオをセッションに追加.
        mySession.addInput(audioInput);
        
        // 出力先を生成.
        myImageOutput = AVCaptureStillImageOutput()
        
        // セッションに追加.
        mySession.addOutput(myImageOutput)
        
        // 動画の保存.
        myVideoOutput = AVCaptureMovieFileOutput()
        
        // ビデオ出力をOutputに追加.
        mySession.addOutput(myVideoOutput)
        
        // 画像を表示するレイヤーを生成.
        let myVideoLayer = AVCaptureVideoPreviewLayer.init(session: mySession)
        myVideoLayer?.frame = self.view.bounds
        myVideoLayer?.videoGravity = AVLayerVideoGravityResizeAspectFill
        
        // Viewに追加.
        self.view.layer.addSublayer(myVideoLayer!)
        
        // セッション開始.
        mySession.startRunning()
        
        // UIボタンを作成.
        myButtonStart = UIButton(frame: CGRect(x: 0, y: 0, width: 120, height: 50))
        myButtonStop = UIButton(frame: CGRect(x: 0, y: 0, width: 120, height: 50))
        
        // 背景色を設定.
        myButtonStart.backgroundColor = UIColor.red
        myButtonStop.backgroundColor = UIColor.gray
        
        // 枠を丸くする.
        myButtonStart.layer.masksToBounds = true
        myButtonStop.layer.masksToBounds = true
        
        // タイトルを設定.
        myButtonStart.setTitle("撮影", for: .normal)
        myButtonStop.setTitle("停止", for: .normal)
        
        // コーナーの半径.
        myButtonStart.layer.cornerRadius = 20.0
        myButtonStop.layer.cornerRadius = 20.0
        
        // ボタンの位置を指定.
        myButtonStart.layer.position = CGPoint(x: self.view.bounds.width/2 - 70, y:self.view.bounds.height-50)
        myButtonStop.layer.position = CGPoint(x: self.view.bounds.width/2 + 70, y:self.view.bounds.height-50)
        
        // イベントを追加.
        myButtonStart.addTarget(self, action: #selector(onClickMyButton(sender:)), for: .touchUpInside)
        myButtonStop.addTarget(self, action: #selector(onClickMyButton(sender:)), for: .touchUpInside)
        
        // UIボタンをViewに追加.
        self.view.addSubview(myButtonStart);
        self.view.addSubview(myButtonStop);
    }
    
    // ボタンイベント.
    func onClickMyButton(sender: UIButton){
        if( sender == myButtonStart ){
            let paths = NSSearchPathForDirectoriesInDomains(.documentDirectory, .userDomainMask, true)
            print("Start!")
            // フォルダ.
            let documentsDirectory = paths[0]
            // ファイル名.
            let filePath = "\(documentsDirectory)/test3.mov"
            // URL.
            let fileURL = URL(fileURLWithPath: filePath)
            // 録画開始.
            myVideoOutput.startRecording(toOutputFileURL: fileURL, recordingDelegate: self)
        } else if ( sender == myButtonStop ){
            print("Stop!")
            myVideoOutput.stopRecording()
        }
    }
    
    
    // MARK: - AVCaptureFileOutputRecordingDelegate
    
    //動画がキャプチャーされた後に呼ばれるメソッド.
    func capture(_ captureOutput: AVCaptureFileOutput!, didFinishRecordingToOutputFileAt outputFileURL: URL!, fromConnections connections: [AnyObject]!, error: Error!) {
        PHPhotoLibrary.shared().performChanges({
            // フォトアプリの中にあるアルバムを検索する.
            let list = PHAssetCollection.fetchAssetCollections(with: PHAssetCollectionType.album, subtype: PHAssetCollectionSubtype.any, options: nil)
            var assetAlbum : PHAssetCollection!
            // リストの中にあるオブジェクトに対して１つずつ呼び出す.
            for i in 0 ..< list.count {
                let item = list.object(at: i)
                // アルバムを検出.
                if item.localizedTitle == "sample" {
                    assetAlbum = item
                    // 検出を止める.
                    break
                }
            }
            // URLからResultFetchを作成.
            let result = PHAssetChangeRequest.creationRequestForAssetFromVideo(atFileURL: outputFileURL)
            // PHAssetを作成.
            let assetPlaceholder = result?.placeholderForCreatedAsset
            // どのアルバムに入れるかを選択.
            let albumChangeRequset = PHAssetCollectionChangeRequest(for: assetAlbum)
            // アルバムにアセットを追加する.
            albumChangeRequset?.addAssets([assetPlaceholder!])
            }, completionHandler: nil)
        
        print("Complete!")
    }
 
}
```

## Swift 2.3

```swift
//
//  ViewController.swift
//  phptokit005
//
//  Copyright © 2016年 FaBo, Inc. All rights reserved.
//
import UIKit
import AVFoundation
import Photos

class ViewController: UIViewController, AVCaptureFileOutputRecordingDelegate {
    
    // セッション.
    var mySession: AVCaptureSession!
    // デバイス.
    var myDevice: AVCaptureDevice!
    // 画像のアウトプット.
    var myImageOutput: AVCaptureStillImageOutput!
    // ビデオのアウトプット.
    var myVideoOutput: AVCaptureMovieFileOutput!
    // スタートボタン.
    var myButtonStart: UIButton!
    // ストップボタン.
    var myButtonStop: UIButton!
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        // セッションの作成.
        mySession = AVCaptureSession()
        // デバイス一覧の取得.
        let devices = AVCaptureDevice.devices()
        
        // バックライトをmyDeviceに格納.
        for device in devices{
            if(device.position == AVCaptureDevicePosition.Back){
                myDevice = device as! AVCaptureDevice
            }
        }
        
        // バックカメラを取得.
        let videoInput = try! AVCaptureDeviceInput.init(device: myDevice)
        
        // ビデオをセッションのInputに追加.
        mySession.addInput(videoInput)
        
        // マイクを取得.
        let audioCaptureDevice = AVCaptureDevice.devicesWithMediaType(AVMediaTypeAudio)
        
        // マイクをセッションのInputに追加.
        let audioInput = try! AVCaptureDeviceInput.init(device: audioCaptureDevice.first as! AVCaptureDevice)
        
        // オーディオをセッションに追加.
        mySession.addInput(audioInput);
        
        // 出力先を生成.
        myImageOutput = AVCaptureStillImageOutput()
        
        // セッションに追加.
        mySession.addOutput(myImageOutput)
        
        // 動画の保存.
        myVideoOutput = AVCaptureMovieFileOutput()
        
        // ビデオ出力をOutputに追加.
        mySession.addOutput(myVideoOutput)
        
        // 画像を表示するレイヤーを生成.
        let myVideoLayer = AVCaptureVideoPreviewLayer.init(session: mySession)
        myVideoLayer.frame = self.view.bounds
        myVideoLayer.videoGravity = AVLayerVideoGravityResizeAspectFill
        
        // Viewに追加.
        self.view.layer.addSublayer(myVideoLayer)
        
        // セッション開始.
        mySession.startRunning()
        
        // UIボタンを作成.
        myButtonStart = UIButton(frame: CGRectMake(0,0,120,50))
        myButtonStop = UIButton(frame: CGRectMake(0,0,120,50))
        
        // 背景色を設定.
        myButtonStart.backgroundColor = UIColor.redColor();
        myButtonStop.backgroundColor = UIColor.grayColor();
        
        // 枠を丸くする.
        myButtonStart.layer.masksToBounds = true
        myButtonStop.layer.masksToBounds = true
        
        // タイトルを設定.
        myButtonStart.setTitle("撮影", forState: .Normal)
        myButtonStop.setTitle("停止", forState: .Normal)
        
        // コーナーの半径.
        myButtonStart.layer.cornerRadius = 20.0
        myButtonStop.layer.cornerRadius = 20.0
        
        // ボタンの位置を指定.
        myButtonStart.layer.position = CGPoint(x: self.view.bounds.width/2 - 70, y:self.view.bounds.height-50)
        myButtonStop.layer.position = CGPoint(x: self.view.bounds.width/2 + 70, y:self.view.bounds.height-50)
        
        // イベントを追加.
        myButtonStart.addTarget(self, action: #selector(ViewController.onClickMyButton(_:)), forControlEvents: .TouchUpInside)
        myButtonStop.addTarget(self, action: #selector(ViewController.onClickMyButton(_:)), forControlEvents: .TouchUpInside)
        
        // UIボタンをViewに追加.
        self.view.addSubview(myButtonStart);
        self.view.addSubview(myButtonStop);
    }
    
    // ボタンイベント.
    func onClickMyButton(sender: UIButton){
        if( sender == myButtonStart ){
            let paths = NSSearchPathForDirectoriesInDomains(.DocumentDirectory, .UserDomainMask, true)
            print("Start!")
            // フォルダ.
            let documentsDirectory = paths[0]
            // ファイル名.
            let filePath = "\(documentsDirectory)/test3.mov"
            // URL.
            let fileURL = NSURL(fileURLWithPath: filePath)
            // 録画開始.
            myVideoOutput.startRecordingToOutputFileURL(fileURL, recordingDelegate: self)
        } else if ( sender == myButtonStop ){
            print("Stop!")
            myVideoOutput.stopRecording()
        }
    }
    
    
    // MARK: - AVCaptureFileOutputRecordingDelegate
    
    // 動画のキャプチャーが終わった時に呼ばれるメソッド.
    func captureOutput(captureOutput: AVCaptureFileOutput!, didFinishRecordingToOutputFileAtURL outputFileURL: NSURL!, fromConnections connections: [AnyObject]!, error: NSError!) {
        PHPhotoLibrary.sharedPhotoLibrary().performChanges({
            // フォトアプリの中にあるアルバムを検索する.
            let list = PHAssetCollection.fetchAssetCollectionsWithType(PHAssetCollectionType.Album, subtype: PHAssetCollectionSubtype.Any, options: nil)
            var assetAlbum : PHAssetCollection!
            // リストの中にあるオブジェクトに対して１つずつ呼び出す.
            list.enumerateObjectsUsingBlock { (album, index, isStop) in
                // アルバムを検出.
                if album.localizedTitle == "sample" {
                    assetAlbum = album as! PHAssetCollection
                    // 検出を止める.
                    isStop.memory = true
                }
            }
            // URLからResultFetchを作成.
            let result = PHAssetChangeRequest.creationRequestForAssetFromVideoAtFileURL(outputFileURL)
            // PHAssetを作成.
            let assetPlaceholder = result?.placeholderForCreatedAsset
            // どのアルバムに入れるかを選択.
            let albumChangeRequset = PHAssetCollectionChangeRequest(forAssetCollection: assetAlbum)
            // アルバムにアセットを追加する.
            albumChangeRequset?.addAssets([assetPlaceholder!])
            }, completionHandler: nil)
        
        print("Complete!")
    }
}
```

## 2.xと3.xの差分

* `AVCaptureDevice.devicesWithMediaType(String!)`から`AVCaptureDevice.devices(withMediaType: String!)`に変更
* `AVCaptureMovieFileOutput.startRecordingToOutputFileURL(NSURL!, recordingDelegate: AVCaptureFileOutputRecordingDelegate!)`から`AVCaptureMovieFileOutput.startRecording(toOutputFileURL: URL!, recordingDelegate: AVCaptureFileOutputRecordingDelegate!)`に変更
* `PHPhotoLibrary.sharedPhotoLibrary()`から`PHPhotoLibrary.shared()`に変更
* `PHAssetCollection.fetchAssetCollectionsWithType(PHAssetCollectionType, subtype: PHAssetCollectionSubtype, options: PHFetchOptions?)`から`PHAssetCollection.fetchAssetCollections(with: PHAssetCollectionType, subtype: PHAssetCollectionSubtype, options: PHFetchOptions?)`に変更
* `NSURL`から`URL`に変更

* 

## Reference

* Photos Framework
    * https://developer.apple.com/library/ios/documentation/Photos/Reference/Photos_Framework/
* PHPhotoLibrary
    * https://developer.apple.com/library/ios/documentation/Photos/Reference/PHPhotoLibrary_Class/
* AV Foundation Framework
    * https://developer.apple.com/library/ios/documentation/AVFoundation/Reference/AVFoundationFramework/
* AVCaptureFileOutputRecordingDelegate
    * https://developer.apple.com/reference/avfoundation/avcapturefileoutputrecordingdelegate
