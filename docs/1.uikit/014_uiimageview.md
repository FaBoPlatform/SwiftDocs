# UIImageViewで画像を表示

![Preview uikit014](./img/uikit014.png)

## Swift 3.0

```swift
//
//  ViewController.swift
//  UIKit014
//
//  Copyright © 2016年 FaBo, Inc. All rights reserved.
//

import UIKit

class ViewController: UIViewController {
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        // 画像を設定する.
        let myImage: UIImage = UIImage(named: "fabo_logo.png")!
        let imageWidth: CGFloat = 300
        let imageHeight: CGFloat = 100
        
        // 画像を縮小する(0.5倍)
        // 表示する座標を設定.
        let downPosX: CGFloat = (self.view.bounds.width - imageWidth) / 2
        let downPosY: CGFloat = 50
        
        // 表示用のUIImageViewを生成.
        let myScaleDownView: UIImageView = UIImageView(frame:  CGRect(x: downPosX, y: downPosY, width: imageWidth, height: imageHeight))
        
        // UIImageViewに画像を設定する.
        myScaleDownView.image = myImage
        
        // 縮小用(0.5倍)のアフィン行列を生成する.
        myScaleDownView.transform = CGAffineTransform(scaleX: 0.5, y: 0.5)
        
        // Viewに追加する.
        self.view.addSubview(myScaleDownView)
        
        
        // 画像を拡大(1.2倍)
        // 表示する座標を設定.
        let upPosX: CGFloat = (self.view.bounds.width - imageWidth) / 2
        let upPosY: CGFloat = 150
        
        // 表示用のUIImageViewを生成.
        let myScaleUpView: UIImageView = UIImageView(frame:  CGRect(x: upPosX, y: upPosY, width: imageWidth, height: imageHeight))
        
        // UIImageViewに画像を設定する.
        myScaleUpView.image = myImage
        
        // 縮小用(0.5倍)のアフィン行列を生成する.
        myScaleUpView.transform = CGAffineTransform(scaleX: 1.2, y: 1.2)
        
        // Viewに追加する.
        self.view.addSubview(myScaleUpView)
        
        
        // 画像を回転する.
        // 表示する座標を設定.
        let rotatePosX: CGFloat = (self.view.bounds.width - imageWidth) / 2
        let rotatePosY: CGFloat = 350
        
        // 表示用のUIImageViewを生成.
        let myRotateView:UIImageView = UIImageView(frame: CGRect(x: rotatePosX, y: rotatePosY, width: imageWidth, height: imageHeight))
        
        // UIImageViewに画像を設定する.
        myRotateView.image = myImage
        
        // radianで回転角度を指定(30度)する.
        let angle: CGFloat = CGFloat((30.0 * M_PI) / 180.0)
        
        // 回転用のアフィン行列を生成する.
        myRotateView.transform = CGAffineTransform(rotationAngle: angle)
        
        // Viewに追加する.
        self.view.addSubview(myRotateView)
        
        
        // 画像を反転する.
        // 表示する座標を設定.
        let reversePosX: CGFloat = (self.view.bounds.width - imageWidth) / 2
        let reversePosY: CGFloat = 550
        
        // 表示用のUIImageViewを生成.
        let myReverseView: UIImageView = UIImageView(frame:  CGRect(x: reversePosX, y: reversePosY, width: imageWidth, height: imageHeight))
        
        // 画像を設定する.
        myReverseView.image = myImage
        
        // 縮小用(0.5倍)のアフィン行列を生成する.
        myReverseView.transform = myReverseView.transform.scaledBy(x: -1.0, y: 1.0)
        
        // Viewに追加する.
        self.view.addSubview(myReverseView)
        
    }
    
    override func didReceiveMemoryWarning() {
        super.didReceiveMemoryWarning()
        // Dispose of any resources that can be recreated.
    }
    
}
```

## Swift 2.3

```swift
//
//  ViewController.swift
//  UIKit014
//
//  Copyright © 2016年 FaBo, Inc. All rights reserved.
//

import UIKit

class ViewController: UIViewController {
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        // 画像を設定する.
        let myImage: UIImage = UIImage(named: "fabo_logo.png")!
        let imageWidth: CGFloat = 300
        let imageHeight: CGFloat = 100
        
        // 画像を縮小する(0.5倍)
        // 表示する座標を設定.
        let downPosX: CGFloat = (self.view.bounds.width - imageWidth) / 2
        let downPosY: CGFloat = 50
        
        // 表示用のUIImageViewを生成.
        let myScaleDownView: UIImageView = UIImageView(frame:  CGRect(x: downPosX, y: downPosY, width: imageWidth, height: imageHeight))
        
        // UIImageViewに画像を設定する.
        myScaleDownView.image = myImage
        
        // 縮小用(0.5倍)のアフィン行列を生成する.
        myScaleDownView.transform = CGAffineTransformMakeScale(0.5, 0.5)
        
        // Viewに追加する.
        self.view.addSubview(myScaleDownView)
        
        
        // 画像を拡大(1.2倍)
        // 表示する座標を設定.
        let upPosX: CGFloat = (self.view.bounds.width - imageWidth) / 2
        let upPosY: CGFloat = 150
        
        // 表示用のUIImageViewを生成.
        let myScaleUpView: UIImageView = UIImageView(frame:  CGRect(x: upPosX, y: upPosY, width: imageWidth, height: imageHeight))
        
        // UIImageViewに画像を設定する.
        myScaleUpView.image = myImage
        
        // 縮小用(0.5倍)のアフィン行列を生成する.
        myScaleUpView.transform = CGAffineTransformMakeScale(1.2, 1.2)
        
        // Viewに追加する.
        self.view.addSubview(myScaleUpView)
        
        
        // 画像を回転する.
        // 表示する座標を設定.
        let rotatePosX: CGFloat = (self.view.bounds.width - imageWidth) / 2
        let rotatePosY: CGFloat = 350
        
        // 表示用のUIImageViewを生成.
        let myRotateView:UIImageView = UIImageView(frame: CGRect(x: rotatePosX, y: rotatePosY, width: imageWidth, height: imageHeight))
        
        // UIImageViewに画像を設定する.
        myRotateView.image = myImage
        
        // radianで回転角度を指定(30度)する.
        let angle: CGFloat = CGFloat((30.0 * M_PI) / 180.0)
        
        // 回転用のアフィン行列を生成する.
        myRotateView.transform = CGAffineTransformMakeRotation(angle)
        
        // Viewに追加する.
        self.view.addSubview(myRotateView)
        
        
        // 画像を反転する.
        // 表示する座標を設定.
        let reversePosX: CGFloat = (self.view.bounds.width - imageWidth) / 2
        let reversePosY: CGFloat = 550
        
        // 表示用のUIImageViewを生成.
        let myReverseView: UIImageView = UIImageView(frame:  CGRect(x: reversePosX, y: reversePosY, width: imageWidth, height: imageHeight))
        
        // 画像を設定する.
        myReverseView.image = myImage
        
        // 縮小用(0.5倍)のアフィン行列を生成する.
        myReverseView.transform = CGAffineTransformScale(myReverseView.transform, -1.0, 1.0)
        
        // Viewに追加する.
        self.view.addSubview(myReverseView)
        
    }
    
    override func didReceiveMemoryWarning() {
        super.didReceiveMemoryWarning()
        // Dispose of any resources that can be recreated.
    }
    
}

```

## 2.3と3.0の差分

* CGAffineTransformの引数定義の変更.


## Reference

* UserNotifications
	* [https://developer.apple.com/reference/uikit/uiimageview](https://developer.apple.com/reference/uikit/uiimageview)