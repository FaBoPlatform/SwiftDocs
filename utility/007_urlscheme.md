# URLスキームによるメーラーの呼び出し

![Preview uikit007_001](img/util007_001.png)
![Preview uikit007_002](img/util007_002.png)

## Swift3.0
```swift
//
//  ViewController.swift
//  Utility007_3.0
//
//  Created by KimikoWatanabe on 2016/08/24.
//  Copyright © 2016年 FaBo, Inc. All rights reserved.
//


```

## Swift 2.3
```swift
//
//  ViewController.swift
//  Utility007_2.3
//
//  Created by KimikoWatanabe on 2016/08/24.
//  Copyright © 2016年 FaBo, Inc. All rights reserved.
//

import UIKit
import SystemConfiguration

class ViewController: UIViewController {

    override func viewDidLoad() {
        super.viewDidLoad()

        //UIボタンを生成.
        let myButton = UIButton(frame: CGRectMake(0,0,100,100))
        myButton.backgroundColor = UIColor.redColor()
        myButton.layer.masksToBounds = true
        myButton.setTitle("メール", forState: .Normal)
        myButton.layer.cornerRadius = 50.0
        myButton.layer.position = CGPoint(x: self.view.bounds.width/2, y: self.view.bounds.height/2)
        myButton.addTarget(self, action: #selector(ViewController.onClickMyButton(_:)), forControlEvents: .TouchUpInside)
        self.view.addSubview(myButton)

    }

    //ボタンイベント.
    func onClickMyButton(sender: UIButton){

        //メーラーのURLを生成.
        let myMailURL : NSURL = NSURL(string:"mailto:1234@xxxx.com")!

        //URLスキームからメーラーを呼び出す.
        UIApplication.sharedApplication().openURL(myMailURL)

    }

}
```

## 2.3と3.0の差分
* UIColorの参照方法が変更(UIColor.grayColor()->UIColor.gray)
* CGRect,CGPointの初期化方法の変更(CGRectMake,CGPointMakeの廃止)

## Reference
* systemConfiguration Framework
* NSURL Class
