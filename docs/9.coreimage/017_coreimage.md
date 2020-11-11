# ディゾルブトランジションフィルタ処理

![Preview coreimage017](./img/coreimage017.png)

```swift fct_label="Swift 5.x/4.x"
//
//  ViewController.swift
//  CoreImage017
//

import UIKit
import CoreImage
import Foundation

extension UIImage{

    /*
     画像をResizeするクラスメソッド.
     */
    class func ResizeÜIImage(image : UIImage,width : CGFloat, height :
        CGFloat)-> UIImage!{

        // 指定された画像の大きさのコンテキストを用意.
        UIGraphicsBeginImageContext(CGSize(width: width, height: height))

        // コンテキストに画像を描画する.
        image.draw(in: CGRect(x: 0, y: 0, width: width, height: height))

        // コンテキストからUIImageを作る.
        let newImage = UIGraphicsGetImageFromCurrentImageContext()

        // コンテキストを閉じる.
        UIGraphicsGetImageFromCurrentImageContext()

        return newImage
    }
}

class ViewController: UIViewController{

    var myImageView:UIImageView!
    var count = 0.0

    // CIDissolveTransitionをインスタンス化.
    let myFilter = CIFilter(name: "CIDissolveTransition")

    // 画像遷移の時間処理.
    @objc func update(timer: Timer) {

        count += 0.1

        // Filterにタイマーをセット.
        myFilter!.setValue(NSNumber(value:count), forKey: kCIInputTimeKey)

        // 加工後の画像を得る.
        let myOutputImage = UIImage(ciImage: myFilter!.outputImage!)

        // 加工後の画像をUIImageViewに設定する.
        myImageView.image = myOutputImage

    }

    override func viewDidLoad() {

        // UIImageViewを作る.
        myImageView = UIImageView(frame: self.view.bounds)
        self.view.addSubview(myImageView)

        // 変換元の画像と遷移の終着点の画像を作る.
        let CIImage1 = CIImage(image: UIImage.ResizeÜIImage(image: UIImage(named:"sample1")!,width: self.view.frame.maxX, height:self.view.frame.maxY))

        let CIImage2 = CIImage(image: UIImage.ResizeÜIImage(image: UIImage(named: "sample2")!, width: self.view.frame.maxX, height:self.view.frame.maxY))

        myFilter!.setValue(CIImage1, forKey: kCIInputImageKey)
        myFilter!.setValue(CIImage2, forKey: kCIInputTargetImageKey)

        // Nstimerの設定.
        Timer.scheduledTimer(timeInterval: 0.1, target: self, selector: #selector(ViewController.update(timer:)), userInfo: nil, repeats: true)

        // 加工後の画像を得る.
        let myOutputImage = UIImage(ciImage: myFilter!.outputImage!)

        // 加工後の画像をUIImageViewに設定する.
        myImageView.image = myOutputImage
    }
}
```

```swift fct_label="Swift 3.x"
//
//  ViewController.swift
//  CoreImage017
//
//  Created by Misato Morino on 2016/09/19.
//  Copyright © 2016年 Misato Morino. All rights reserved.
//

import UIKit
import CoreImage
import Foundation

extension UIImage{
    
    /*
     画像をResizeするクラスメソッド.
     */
    class func ResizeÜIImage(image : UIImage,width : CGFloat, height :
        CGFloat)-> UIImage!{
        
        // 指定された画像の大きさのコンテキストを用意.
        UIGraphicsBeginImageContext(CGSize(width: width, height: height))
        
        // コンテキストに画像を描画する.
        image.draw(in: CGRect(x: 0, y: 0, width: width, height: height))
        
        // コンテキストからUIImageを作る.
        let newImage = UIGraphicsGetImageFromCurrentImageContext()
        
        // コンテキストを閉じる.
        UIGraphicsGetImageFromCurrentImageContext()
        
        return newImage
    }
}

class ViewController: UIViewController{
    
    var myImageView:UIImageView!
    var count = 0.0
    var myTime = NSNumber(value:0.0)
    
    // CIDissolveTransitionをインスタンス化.
    let myFilter = CIFilter(name: "CIDissolveTransition")
    
    // 画像遷移の時間処理.
    func update(timer: Timer) {
        
        count += 0.1
        myTime = NSNumber(value: count)
        
        // Filterにタイマーをセット.
        myFilter!.setValue(myTime, forKey: kCIInputTimeKey)
        
        // 加工後の画像を得る.
        let myOutputImage = UIImage(ciImage: myFilter!.outputImage!)
        
        // 加工後の画像をUIImageViewに設定する.
        myImageView.image = myOutputImage
        
    }
    
    override func viewDidLoad() {
        
        // UIImageViewを作る.
        myImageView = UIImageView(frame: self.view.bounds)
        self.view.addSubview(myImageView)
        
        // 変換元の画像と遷移の終着点の画像を作る.
        let CIImage1 = CIImage(image: UIImage.ResizeÜIImage(image: UIImage(named:"sample1")!,width: self.view.frame.maxX, height:self.view.frame.maxY))
        
        let CIImage2 = CIImage(image: UIImage.ResizeÜIImage(image: UIImage(named: "sample2")!, width: self.view.frame.maxX, height:self.view.frame.maxY))
        
        myFilter!.setValue(CIImage1, forKey: kCIInputImageKey)
        myFilter!.setValue(CIImage2, forKey: kCIInputTargetImageKey)
        
        // Nstimerの設定.
        Timer.scheduledTimer(timeInterval: 0.001, target: self, selector: #selector(ViewController.update(timer:)), userInfo: nil, repeats: true)
        
        // 加工後の画像を得る.
        let myOutputImage = UIImage(ciImage: myFilter!.outputImage!)
        
        // 加工後の画像をUIImageViewに設定する.
        myImageView.image = myOutputImage
    }
} 
```

```swift fct_label="Swift 2.x"
//
//  ViewController.swift
//  CoreImage017
//
//  Created by Misato Morino on 2016/09/19.
//  Copyright © 2016年 Misato Morino. All rights reserved.
//

import UIKit
import CoreImage
import Foundation

extension UIImage{
    
    /*
     画像をResizeするクラスメソッド.
     */
    class func ResizeÜIImage(image : UIImage,width : CGFloat, height :
        CGFloat)-> UIImage!{
        
        // 指定された画像の大きさのコンテキストを用意.
        UIGraphicsBeginImageContext(CGSizeMake(width, height))
        
        // コンテキストに画像を描画する.
        image.drawInRect(CGRectMake(0, 0, width, height))
        
        // コンテキストからUIImageを作る.
        let newImage = UIGraphicsGetImageFromCurrentImageContext()
        
        // コンテキストを閉じる.
        UIGraphicsGetImageFromCurrentImageContext()
        
        return newImage
    }
}

class ViewController: UIViewController{
    
    var myImageView:UIImageView!
    var count = 0.0
    var myTime = NSNumber(double:0.0)
    
    // CIDissolveTransitionをインスタンス化.
    let myFilter = CIFilter(name: "CIDissolveTransition")
    
    // 画像遷移の時間処理.
    func update(timer: NSTimer) {
        
        count += 0.1
        myTime = count
        
        // Filterにタイマーをセット.
        myFilter!.setValue(myTime, forKey: kCIInputTimeKey)
        
        // 加工後の画像を得る.
        let myOutputImage = UIImage(CIImage: myFilter!.outputImage!)
        
        // 加工後の画像をUIImageViewに設定する.
        myImageView.image = myOutputImage
        
    }
    
    override func viewDidLoad() {
        
        // UIImageViewを作る.
        myImageView = UIImageView(frame: self.view.bounds)
        self.view.addSubview(myImageView)
        
        // 変換元の画像と遷移の終着点の画像を作る.
        let CIImage1 = CIImage(image: UIImage.ResizeÜIImage(UIImage(named:"sample1")!,width: self.view.frame.maxX, height:self.view.frame.maxY))
        
        let CIImage2 = CIImage(image: UIImage.ResizeÜIImage(UIImage(named: "sample2")!, width: self.view.frame.maxX, height:self.view.frame.maxY))
        
        myFilter!.setValue(CIImage1, forKey: kCIInputImageKey)
        myFilter!.setValue(CIImage2, forKey: kCIInputTargetImageKey)
        
        // Nstimerの設定.
        NSTimer.scheduledTimerWithTimeInterval(0.001, target: self, selector: #selector(ViewController.update(_:)), userInfo: nil, repeats: true)
        
        // 加工後の画像を得る.
        let myOutputImage = UIImage(CIImage: myFilter!.outputImage!)
        
        // 加工後の画像をUIImageViewに設定する.
        myImageView.image = myOutputImage
    }
}
```

## 3.xと4.xの差分
* `func update(timer: NSTimer)` に `@objc` を付加

## 2.xと3.xの差分
* `init(CIImage:)` から `init(ciImage:)` に変更
* `scheduledTimerWithTimeInterval` から `scheduledTimer` に変更

## Reference

* CIFilter
    * [https://developer.apple.com/reference/coreimage/cifilter](https://developer.apple.com/reference/coreimage/cifilter)
* CIImage
    * [https://developer.apple.com/reference/coreimage/ciimage](https://developer.apple.com/reference/coreimage/ciimage)
