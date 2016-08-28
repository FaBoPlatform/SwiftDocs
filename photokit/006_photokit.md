# アルバムのアセットを削除する

![Preview photokit006](./img/PhotoKit005.png) ![Preview PhotoKit006_2](./img/PhotoKit006_2.png) ![Preview PhotoKit006_3](./img/PhotoKit006_3.png) ![Preview PhotoKit006_4](./img/PhotoKit006_4.png)

## Swift 3.0

`Info.plist`に`NSPhotoLibraryUsageDescription`を追加します

```swift
//
//  ViewController.swift
//  phptokit001
//
//  Copyright © 2016年 FaBo, Inc. All rights reserved.
//
import UIKit
import Photos

class ViewController: UIViewController {
    
    override func viewDidLoad() {
        
        // ユーザーに許可を促す.
        PHPhotoLibrary.requestAuthorization({ status in
            switch status {
            case .authorized:
                print("Authorized")
            case .denied:
                print("Denied")
            case .notDetermined:
                print("NotDetermined")
            case .restricted:
                print("Restricted")
            }
        })
    }
}
```

## Swift 2.3

```swift
//
//  ViewController.swift
//  phptokit001
//
//  Copyright © 2016年 FaBo, Inc. All rights reserved.
//
import UIKit
import Photos

class ViewController: UIViewController {
    
    override func viewDidLoad() {
        
        // ユーザーに許可を促す.
        PHPhotoLibrary.requestAuthorization({ status in
            switch status {
            case .Authorized:
                print("Authorized")
            case .Denied:
                print("Denied")
            case .NotDetermined:
                print("NotDetermined")
            case .Restricted:
                print("Restricted")
            }
        })
    }
}
```

## 2.xと3.xの差分

* enum列挙体`PHAuthorizationStatus`のイニシャルが小文字に変更

## Reference

* Photos Framework
    * https://developer.apple.com/library/ios/documentation/Photos/Reference/Photos_Framework/
* PHPhotoLibrary
    * https://developer.apple.com/library/ios/documentation/Photos/Reference/PHPhotoLibrary_Class/
