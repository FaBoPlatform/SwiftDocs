#

![Preview uikit0]()

## Swift3.0
```swift
//
//  ViewController.swift
//  UIKit044_3.0
//
//  Created by KimikoWatanabe on 2016/08/18.
//  Copyright © 2016年 FaBo, Inc. All rights reserved.
//


```

## Swift 2.3
```swift
//
//  ViewController.swift
//  UIKit044_2.3
//
//  Created by KimikoWatanabe on 2016/08/18.
//  Copyright © 2016年 FaBo, Inc. All rights reserved.
//

import UIKit

class ViewController: UIViewController {

    var mySecondWindow: UIWindow!

    override func viewDidLoad() {
        super.viewDidLoad()
        let screen: NSArray = UIScreen.screens()

        // 接続中の画面が2つある場合、2番目を表示.
        if screen.count > 1 {
            let screen: UIScreen = UIScreen.screens()[1]
            showSecondScreenWindow(screen)
        }

        // 画面の接続がおこなわれた時に発生するNotification.
        let myDefaultCenter: NSNotificationCenter = NSNotificationCenter.defaultCenter()
        myDefaultCenter.addObserver(self, selector: #selector(ViewController.screenDidConnect(_:)), name: UIScreenDidConnectNotification, object: nil)
        myDefaultCenter.addObserver(self, selector: #selector(ViewController.screenDidDisconnect(_:)), name: UIScreenDidDisconnectNotification, object: nil)
        myDefaultCenter.addObserver(self, selector: #selector(ViewController.screenDidModeChange(_:)), name: UIScreenModeDidChangeNotification, object: nil)

        // Labelを作成.
        let myLabel: UILabel = UILabel(frame: CGRectMake(0,0,200,50))
        myLabel.backgroundColor = UIColor.orangeColor()
        myLabel.layer.masksToBounds = true
        myLabel.layer.cornerRadius = 20.0
        myLabel.text = "Hello 1st Screen"
        myLabel.textColor = UIColor.whiteColor()
        myLabel.shadowColor = UIColor.grayColor()
        myLabel.textAlignment = NSTextAlignment.Center
        myLabel.layer.position = CGPoint(x: self.view.bounds.width/2,y: 200)
        self.view.addSubview(myLabel)

    }


    override func didReceiveMemoryWarning() {
        super.didReceiveMemoryWarning()
    }

    /*
     画面の接続された時.
     */
    func screenDidConnect(notification: NSNotification){

        print("screenDidConnect")

        let screen: NSArray = UIScreen.screens()

        if screen.count > 1 {
            let screen: UIScreen = UIScreen.screens()[1]
            showSecondScreenWindow(screen)
        }
    }

    /*
     画面が切断された時.
     */
    func screenDidDisconnect(notification: NSNotification){
        print("screenDidDisconnect")
    }

    /*
     画面のモードが変わった時.
     */
    func screenDidModeChange(notification: NSNotification){
        print("screenDidModeChange")
    }

    /*
     2画面目の画面のレイアウト.
     */
    func showSecondScreenWindow(screen: UIScreen!){

        mySecondWindow = UIWindow()
        mySecondWindow.screen = screen
        mySecondWindow.frame = CGRectMake(0, 0, screen.bounds.width, screen.bounds.height)
        mySecondWindow.backgroundColor = UIColor.redColor()
        self.mySecondWindow.makeKeyAndVisible()

        // Labelを作成.
        let myLabel: UILabel = UILabel(frame: CGRectMake(0,0,200,50))
        myLabel.backgroundColor = UIColor.orangeColor()
        myLabel.layer.masksToBounds = true
        myLabel.layer.cornerRadius = 20.0
        myLabel.text = "Hello Second Screen"
        myLabel.textColor = UIColor.whiteColor()
        myLabel.shadowColor = UIColor.grayColor()
        myLabel.textAlignment = NSTextAlignment.Center
        myLabel.layer.position = CGPoint(x: self.mySecondWindow.bounds.width/2,y: 200)
        self.mySecondWindow.addSubview(myLabel)
    }
}
```

## 2.3と3.0の差分
* UIColorの参照方法が変更(UIColor.grayColor()->UIColor.gray)
* CGRect,CGPointの初期化方法の変更(CGRectMake,CGPointMakeの廃止)

## Reference
* UIScreen Class
