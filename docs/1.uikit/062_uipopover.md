# UIPopoverControllerを使ってViewをポップさせる(iPad専用)

![Preview uikit062](img/uikit062.png)

```swift fct_label="Swift 4.x"

//
//  ViewController.swift
//  swiftdocs
//
//  Created by Misato Morino on 2016/08/15.
//  Copyright © 2016年 Misato Morino. All rights reserved.
//

import UIKit

class ViewController: UIViewController {
    
    override func viewDidLoad() {
        
        // Buttonを作る.
        let myButton = UIButton(frame: CGRect(x: 0, y: 0, width: 200, height: 50))
        myButton.layer.position = self.view.center
        myButton.layer.masksToBounds = true
        myButton.layer.cornerRadius = 20.0
        myButton.backgroundColor = UIColor.orange
        myButton.setTitle("Present!", for: UIControl.State.normal)
        myButton.addTarget(self, action: #selector(ViewController.onMyButtonClick(sender:)), for: UIControl.Event.touchUpInside)
        
        // viewにButtonを追加.
        self.view.addSubview(myButton)
    }
    
    /*
     ボタンがタップされた時に呼び出されるメソッド.
     */
    @objc func onMyButtonClick(sender : UIButton){
        
        // コンテンツのViewControllerを生成.
        let popover = UIViewController()
        
        // コンテンツViewControllerのサイズを指定.
        popover.preferredContentSize = CGSize(width: 200, height: 200)
        
        // コンテンツViewControllerの背景を青色に設定.
        popover.view.backgroundColor = UIColor.blue
        popover.modalPresentationStyle = .popover
        
        if let presentationController = popover.popoverPresentationController {
            presentationController.permittedArrowDirections = .any
            presentationController.sourceView = sender
            presentationController.sourceRect = sender.bounds
        }
        
        present(popover, animated: true, completion: nil)
    }
}
```

```swift fct_label="Swift 3.x"
//
//  ViewController.swift
//  swiftdocs
//
//  Created by Misato Morino on 2016/08/15.
//  Copyright © 2016年 Misato Morino. All rights reserved.
//

import UIKit

class ViewController: UIViewController {
    
    override func viewDidLoad() {
        
        // Buttonを作る.
        let myButton = UIButton(frame: CGRect(x: 0, y: 0, width: 200, height: 50))
        myButton.layer.position = self.view.center
        myButton.layer.masksToBounds = true
        myButton.layer.cornerRadius = 20.0
        myButton.backgroundColor = UIColor.orange
        myButton.setTitle("Present!", for: UIControlState.normal)
        myButton.addTarget(self, action: #selector(ViewController.onMyButtonClick(sender:)), for: UIControlEvents.touchUpInside)
        
        // viewにButtonを追加.
        self.view.addSubview(myButton)
    }
    
    /*
     ボタンがタップされた時に呼び出されるメソッド.
     */
    func onMyButtonClick(sender : UIButton){
        
        // コンテンツのViewControllerを生成.
        let popover = UIViewController()
        
        // コンテンツViewControllerのサイズを指定.
        popover.preferredContentSize = CGSize(width: 200, height: 200)
        
        // コンテンツViewControllerの背景を青色に設定.
        popover.view.backgroundColor = UIColor.blue
        popover.modalPresentationStyle = .popover
        
        if let presentationController = popover.popoverPresentationController {
            presentationController.permittedArrowDirections = .any
            presentationController.sourceView = sender
            presentationController.sourceRect = sender.bounds
        }
        
        present(popover, animated: true, completion: nil)
    } 
} 
``` 

```swift fct_label="Swift 2.3"
//
//  ViewController.swift
//  UIKit062
//
//  Created by Misato Morino on 2016/08/15.
//  Copyright © 2016年 Misato Morino. All rights reserved.
//

import UIKit

class ViewController: UIViewController {
    
    override func viewDidLoad() {
        
        // Buttonを作る.
        let myButton = UIButton(frame: CGRectMake(0, 0, 200, 50))
        myButton.layer.position = self.view.center
        myButton.layer.masksToBounds = true
        myButton.layer.cornerRadius = 20.0
        myButton.backgroundColor = UIColor.orangeColor()
        myButton.setTitle("Present!", forState: UIControlState.Normal)
        myButton.addTarget(self, action: #selector(ViewController.onMyButtonClick(_:)), forControlEvents: UIControlEvents.TouchUpInside)
        
        // viewにButtonを追加.
        self.view.addSubview(myButton)
    }
    
    /*
     ボタンがタップされた時に呼び出されるメソッド.
     */
    func onMyButtonClick(sender : UIButton){
        
        // コンテンツのViewControllerを生成.
        let popover = UIViewController()
        
        // コンテンツViewControllerのサイズを指定.
        popover.preferredContentSize = CGSize(width: 200, height: 200)
        
        // コンテンツViewControllerの背景を青色に設定.
        popover.view.backgroundColor = UIColor.blueColor()
        popover.modalPresentationStyle = .Popover
        
        if let presentationController = popover.popoverPresentationController {
            presentationController.permittedArrowDirections = .Any
            presentationController.sourceView = sender
            presentationController.sourceRect = sender.bounds
        }
        
        presentViewController(popover, animated: true, completion: nil)
    } 
}
``` 

## 3.xと4.xの差分
* ```UIControlState``` が ```UIControl.State``` に変更
* ```UIControlEvents``` が ```UIControl.Event``` に変更
* ```func onMyButtonClick(sender : UIButton)``` に ```@objc``` を追加

## 2.3と3.0の差分

* CGRectMakeが廃止
* ```presentViewController``` から ```present``` に変更

## Reference

* UIViewController
    * [https://developer.apple.com/reference/uikit/uiviewcontroller](https://developer.apple.com/reference/uikit/uiviewcontroller)
* UIPopverController
    * [https://developer.apple.com/reference/uikit/uipopovercontroller](https://developer.apple.com/reference/uikit/uipopovercontroller)
