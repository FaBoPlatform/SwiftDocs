# Facebookに投稿

![Preview social002](img/social002_001.png) ![Preview social002](img/social002_002.png)

```swift fct_label="Swift 5.x/4.x"
//
//  ViewController.swift
//  Social002
//
//  Created by Misato Morino on 2016/08/15.
//  Copyright © 2016年 Misato Morino. All rights reserved.
//

import UIKit
import Social

class ViewController: UIViewController {
    
    var myComposeView : SLComposeViewController!
    
    override func viewDidLoad() {
        
        super.viewDidLoad()
        
        // Facebookカラーの作成.
        let hex: Int = 0x3B5998
        let red = Double((hex & 0xFF0000) >> 16) / 255.0
        let green = Double((hex & 0xFF00) >> 8) / 255.0
        let blue = Double((hex & 0xFF)) / 255.0
        let myColor: UIColor = UIColor( red: CGFloat(red), green: CGFloat(green), blue: CGFloat(blue), alpha: CGFloat(1.0))
        
        // Facebook用のUIButtonの生成.
        let myFacebookButton = UIButton(frame: CGRect(x: 0, y: 0, width: 100, height: 100))
        myFacebookButton.backgroundColor = myColor
        myFacebookButton.setTitle("Facebook", for: UIControl.State.normal)
        myFacebookButton.setTitleColor(UIColor.white, for: UIControl.State.normal)
        myFacebookButton.addTarget(self, action: #selector(ViewController.postToFacebook(sender:)), for: UIControl.Event.touchUpInside)
        myFacebookButton.layer.cornerRadius = 50.0
        myFacebookButton.layer.position = CGPoint(x: self.view.frame.width/2, y: self.view.frame.height/2)
        
        // buttonをviewに追加.
        self.view.addSubview(myFacebookButton)
    }
    
    // ボタンイベント.
    @objc func postToFacebook(sender : UIButton) {
        
        // postToTwitterと同様.
        // ServiceTypeをFacebookに指定.
        myComposeView = SLComposeViewController(forServiceType: SLServiceTypeFacebook)
        
        // 投稿するテキストを指定.
        myComposeView.setInitialText("Facebook Test")
        
        // 投稿する画像を指定.
        myComposeView.add(UIImage(named: "sample1.jpg"))
        
        // myComposeViewの画面遷移.
        self.present(myComposeView, animated: true, completion: nil)
    }
    
}
```

```swift fct_label="Swift 3.x"
//
//  ViewController.swift
//  Social002
//
//  Created by Misato Morino on 2016/08/15.
//  Copyright © 2016年 Misato Morino. All rights reserved.
//

import UIKit
import Social

class ViewController: UIViewController {
    
    var myComposeView : SLComposeViewController!
    
    override func viewDidLoad() {
        
        super.viewDidLoad()
        
        // Facebookカラーの作成.
        let hex: Int = 0x3B5998
        let red = Double((hex & 0xFF0000) >> 16) / 255.0
        let green = Double((hex & 0xFF00) >> 8) / 255.0
        let blue = Double((hex & 0xFF)) / 255.0
        let myColor: UIColor = UIColor( red: CGFloat(red), green: CGFloat(green), blue: CGFloat(blue), alpha: CGFloat(1.0))
        
        // Facebook用のUIButtonの生成.
        let myFacebookButton = UIButton(frame: CGRect(x: 0, y: 0, width: 100, height: 100))
        myFacebookButton.backgroundColor = myColor
        myFacebookButton.setTitle("Facebook", for: UIControlState.normal)
        myFacebookButton.setTitleColor(UIColor.white, for: UIControlState.normal)
        myFacebookButton.addTarget(self, action: #selector(ViewController.postToFacebook(sender:)), for: UIControlEvents.touchUpInside)
        myFacebookButton.layer.cornerRadius = 50.0
        myFacebookButton.layer.position = CGPoint(x: self.view.frame.width/2, y: self.view.frame.height/2)
        
        // buttonをviewに追加.
        self.view.addSubview(myFacebookButton)
    }
    
    // ボタンイベント.
    func postToFacebook(sender : UIButton) {
        
        // postToTwitterと同様.
        // ServiceTypeをFacebookに指定.
        myComposeView = SLComposeViewController(forServiceType: SLServiceTypeFacebook)
        
        // 投稿するテキストを指定.
        myComposeView.setInitialText("Facebook Test")
        
        // 投稿する画像を指定.
        myComposeView.add(UIImage(named: "sample1.jpg"))
        
        // myComposeViewの画面遷移.
        self.present(myComposeView, animated: true, completion: nil)
    }
    
} 
```

```swift fct_label="Swift 2.3"
//
//  ViewController.swift
//  Social002
//
//  Created by Misato Morino on 2016/08/15.
//  Copyright © 2016年 Misato Morino. All rights reserved.
//

import UIKit
import Social

class ViewController: UIViewController {
    
    var myComposeView : SLComposeViewController!
    
    override func viewDidLoad() {
        
        super.viewDidLoad()
        
        // Facebookカラーの作成.
        let hex: Int = 0x3B5998
        let red = Double((hex & 0xFF0000) >> 16) / 255.0
        let green = Double((hex & 0xFF00) >> 8) / 255.0
        let blue = Double((hex & 0xFF)) / 255.0
        let myColor: UIColor = UIColor( red: CGFloat(red), green: CGFloat(green), blue: CGFloat(blue), alpha: CGFloat(1.0))
        
        // Facebook用のUIButtonの生成.
        let myFacebookButton = UIButton(frame: CGRectMake(0, 0, 100, 100))
        myFacebookButton.backgroundColor = myColor
        myFacebookButton.setTitle("Facebook", forState: UIControlState.Normal)
        myFacebookButton.setTitleColor(UIColor.whiteColor(), forState: UIControlState.Normal)
        myFacebookButton.addTarget(self, action: #selector(ViewController.postToFacebook(_:)), forControlEvents: UIControlEvents.TouchUpInside)
        myFacebookButton.layer.cornerRadius = 50.0
        myFacebookButton.layer.position = CGPointMake(self.view.frame.width/2, self.view.frame.height/2)
        
        // buttonをviewに追加.
        self.view.addSubview(myFacebookButton)
    }
    
    // ボタンイベント.
    func postToFacebook(sender : UIButton) {
        
        // postToTwitterと同様.
        // ServiceTypeをFacebookに指定.
        myComposeView = SLComposeViewController(forServiceType: SLServiceTypeFacebook)
        
        // 投稿するテキストを指定.
        myComposeView.setInitialText("Facebook Test")
        
        // 投稿する画像を指定.
        myComposeView.addImage(UIImage(named: "sample1.jpg"))
        
        // myComposeViewの画面遷移.
        self.presentViewController(myComposeView, animated: true, completion: nil)
    }
    
}
```

## 3.xと4.xの差分
* ```UIControlState``` が ```UIControl.State``` に変更
* ```UIControlEvents``` が ```UIControl.Event``` に変更
* ```func postToFacebook(sender : UIButton)``` に ```@objc``` を追加
* ```social.framework``` が、iOS 11.0から使用不可
* ```SLServiceTypeFacebook``` が、iOS 11.0から非推奨

## 2.3と3.0の差分
    
* ```addImage``` から ```add``` へ変更


## Reference

* SLComposeViewController
    * [https://developer.apple.com/reference/social/slcomposeviewcontroller](https://developer.apple.com/reference/social/slcomposeviewcontroller)
