# ぼかし処理

![Preview coreimage006](./img/coreimage006.png)

```swift fct_label="Swift 5.x/4.x"
//
//  ViewController.swift
//  CoreImage006
//
//  Created by Misato Morino on 2016/08/15.
//  Copyright © 2016年 Misato Morino. All rights reserved.
//

import UIKit
import CoreImage

class ViewController: UIViewController {
    
    // ベース画像.
    let myInputImage = CIImage(image: UIImage(named: "yaruki_moeru_man.png")!)
    
    // ボタン.
    let myButton: UIButton = UIButton()
    
    // UIView
    var myImageView: UIImageView!
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        // UIImageに変換.
        let myInputUIImage: UIImage = UIImage(ciImage: myInputImage!)
        
        // ImageView.
        myImageView = UIImageView(frame: CGRect(x: 0, y: 0, width: myInputUIImage.size.width, height: myInputUIImage.size.height))
        
        // UIImageViewの生成.
        myImageView.image = myInputUIImage
        self.view.addSubview(myImageView)
        
        // ボタン.
        myButton.frame = CGRect(x: 0, y: 0, width: 80, height: 80)
        myButton.backgroundColor = UIColor.blue
        myButton.layer.masksToBounds = true
        myButton.setTitle("ぼかし", for: UIControl.State.normal)
        myButton.titleLabel?.font = UIFont.systemFont(ofSize: UIFont.smallSystemFontSize)
        myButton.setTitleColor(UIColor.white, for: UIControl.State.normal)
        myButton.layer.cornerRadius = 40.0
        myButton.layer.position = CGPoint(x: self.view.frame.width/2, y:self.view.frame.height - 50)
        myButton.tag = 1
        myButton.addTarget(self, action: #selector(ViewController.onClickMyButton(sender:)), for: .touchUpInside)
        
        // 背景色を黒.
        self.view.backgroundColor = UIColor.black
        
        // UIボタンをViewに追加.
        self.view.addSubview(myButton);
    }
    
    // ボタンイベント..
    @objc func onClickMyButton(sender: UIButton){
        
        // CIFilterを生成。nameにどんなを処理するのか記入.
        let myBlurFilter = CIFilter(name: "CIGaussianBlur")
        
        // ばかし処理をいれたい画像をセット.
        myBlurFilter!.setValue(myInputImage, forKey: kCIInputImageKey)
        
        // フィルターを通した画像をアウトプット.
        let myOutputImage : CIImage = myBlurFilter!.outputImage!
        
        // UIImageに変換.
        let myOutputUIImage: UIImage = UIImage(ciImage: myOutputImage)
        
        // 再びUIViewにセット.
        myImageView.image = myOutputUIImage
        
        // 再描画.
        myImageView.setNeedsDisplay()
        
    }
}
```

```swift fct_label="Swift 3.x"
//
//  ViewController.swift
//  CoreImage006
//
//  Created by Misato Morino on 2016/08/15.
//  Copyright © 2016年 Misato Morino. All rights reserved.
//

import UIKit
import CoreImage

class ViewController: UIViewController {
    
    // ベース画像.
    let myInputImage = CIImage(image: UIImage(named: "sample1")!)
    
    // ボタン.
    let myButton: UIButton = UIButton()
    
    // UIView
    var myImageView: UIImageView!
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        // UIImageに変換.
        let myInputUIImage: UIImage = UIImage(ciImage: myInputImage!)
        
        // ImageView.
        myImageView = UIImageView(frame: CGRect(x: 0, y: 0, width: myInputUIImage.size.width, height: myInputUIImage.size.height))
        
        // UIImageViewの生成.
        myImageView.image = myInputUIImage
        self.view.addSubview(myImageView)
        
        // ボタン.
        myButton.frame = CGRect(x: 0, y: 0, width: 80, height: 80)
        myButton.backgroundColor = UIColor.blue
        myButton.layer.masksToBounds = true
        myButton.setTitle("ぼかし", for: UIControlState.normal)
        myButton.titleLabel?.font = UIFont.systemFont(ofSize: UIFont.smallSystemFontSize)
        myButton.setTitleColor(UIColor.white, for: UIControlState.normal)
        myButton.layer.cornerRadius = 40.0
        myButton.layer.position = CGPoint(x: self.view.frame.width/2, y:self.view.frame.height - 50)
        myButton.tag = 1
        myButton.addTarget(self, action: #selector(ViewController.onClickMyButton(sender:)), for: .touchUpInside)
        
        // 背景色を黒.
        self.view.backgroundColor = UIColor.black
        
        // UIボタンをViewに追加.
        self.view.addSubview(myButton);
    }
    
    // ボタンイベント..
    func onClickMyButton(sender: UIButton){
        
        // CIFilterを生成。nameにどんなを処理するのか記入.
        let myBlurFilter = CIFilter(name: "CIGaussianBlur")
        
        // ばかし処理をいれたい画像をセット.
        myBlurFilter!.setValue(myInputImage, forKey: kCIInputImageKey)
        
        // フィルターを通した画像をアウトプット.
        let myOutputImage : CIImage = myBlurFilter!.outputImage!
        
        // UIImageに変換.
        let myOutputUIImage: UIImage = UIImage(ciImage: myOutputImage)
        
        // 再びUIViewにセット.
        myImageView.image = myOutputUIImage
        
        // 再描画.
        myImageView.setNeedsDisplay()
        
    } 
}
```

```swift fct_label="Swift 2.x"
//
//  ViewController.swift
//  CoreImage006
//
//  Created by Misato Morino on 2016/08/15.
//  Copyright © 2016年 Misato Morino. All rights reserved.
//

import UIKit
import CoreImage

class ViewController: UIViewController {
    
    // ベース画像.
    let myInputImage = CIImage(image: UIImage(named: "sample1")!)
    
    // ボタン.
    let myButton: UIButton = UIButton()
    
    // UIView
    var myImageView: UIImageView!
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        // UIImageに変換.
        let myInputUIImage: UIImage = UIImage(CIImage: myInputImage!)
        
        // ImageView.
        myImageView = UIImageView(frame: CGRectMake(0, 0, myInputUIImage.size.width, myInputUIImage.size.height))
        
        // UIImageViewの生成.
        myImageView.image = myInputUIImage
        self.view.addSubview(myImageView)
        
        // ボタン.
        myButton.frame = CGRectMake(0,0,80,80)
        myButton.backgroundColor = UIColor.blueColor();
        myButton.layer.masksToBounds = true
        myButton.setTitle("ぼかし", forState: UIControlState.Normal)
        myButton.titleLabel?.font = UIFont.systemFontOfSize(UIFont.smallSystemFontSize())
        myButton.setTitleColor(UIColor.whiteColor(), forState: UIControlState.Normal)
        myButton.layer.cornerRadius = 40.0
        myButton.layer.position = CGPoint(x: self.view.frame.width/2, y:self.view.frame.height - 50)
        myButton.tag = 1
        myButton.addTarget(self, action: #selector(ViewController.onClickMyButton(_:)), forControlEvents: .TouchUpInside)
        
        // 背景色を黒.
        self.view.backgroundColor = UIColor.blackColor()
        
        // UIボタンをViewに追加.
        self.view.addSubview(myButton);
    }
    
    // ボタンイベント..
    func onClickMyButton(sender: UIButton){
        
        // CIFilterを生成。nameにどんなを処理するのか記入.
        let myBlurFilter = CIFilter(name: "CIGaussianBlur")
        
        // ばかし処理をいれたい画像をセット.
        myBlurFilter!.setValue(myInputImage, forKey: kCIInputImageKey)
        
        // フィルターを通した画像をアウトプット.
        let myOutputImage : CIImage = myBlurFilter!.outputImage!
        
        // UIImageに変換.
        let myOutputUIImage: UIImage = UIImage(CIImage: myOutputImage)
        
        // 再びUIViewにセット.
        myImageView.image = myOutputUIImage
        
        // 再描画.
        myImageView.setNeedsDisplay()
        
    } 
}
```

## 3.xと4.xの差分
* `UIControlState` が `UIControl.State` に変更
* `func onClickMyButton(sender: UIButton)` に `@objc` を追加

## 2.xと3.xの差分
* ```init(CIImage:)``` から ```init(ciImage:)``` に変更

## Reference

* CIFilter
    * [https://developer.apple.com/reference/coreimage/cifilter](https://developer.apple.com/reference/coreimage/cifilter)
* CIImage
    * [https://developer.apple.com/reference/coreimage/ciimage](https://developer.apple.com/reference/coreimage/ciimage)
