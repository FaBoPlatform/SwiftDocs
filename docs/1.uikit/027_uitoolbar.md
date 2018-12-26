# UIToolBarの表示

![Preview uikit027_001](img/uikit027_001.png)
![Preview uikit027_002](img/uikit027_002.png)
![Preview uikit027_003](img/uikit027_003.png)

```swift fct_label="Swift 4.x"
//
//  ViewController.swift
//  UIKit027_4.0
//
//  Created by KimikoWatanabe on 2016/08/13.
//  Copyright © 2016年 FaBo, Inc. All rights reserved.
//

import UIKit

class ViewController: UIViewController, UIToolbarDelegate{

    private var myToolbar: UIToolbar!

    override func viewDidLoad() {
        super.viewDidLoad()

        // 背景を青色に変更する.
        self.view.backgroundColor = UIColor.cyan

        // ツールバーのサイズを決める.
        myToolbar = UIToolbar(frame: CGRect(x:0, y:self.view.bounds.size.height - 44, width:self.view.bounds.size.width, height:40.0))

        // ツールバーの位置を決める.
        myToolbar.layer.position = CGPoint(x: self.view.bounds.width/2, y: self.view.bounds.height-20.0)

        // ツールバーの色を決める.
        myToolbar.barStyle = .blackTranslucent
        myToolbar.tintColor = UIColor.white
        myToolbar.backgroundColor = UIColor.black

        // ボタン１を生成する.
        let myUIBarButtonGreen: UIBarButtonItem = UIBarButtonItem(title: "Green", style:.plain, target: self, action: #selector(ViewController.onClickBarButton(sender:)))
        myUIBarButtonGreen.tag = 1

        // ボタン２を生成する.
        let myUIBarButtonBlue: UIBarButtonItem = UIBarButtonItem(title: "Blue", style:.plain, target: self, action: #selector(ViewController.onClickBarButton(sender:)))
        myUIBarButtonBlue.tag = 2

        // ボタン3を生成する.
        let myUIBarButtonRed: UIBarButtonItem = UIBarButtonItem(title: "Red", style:.plain, target: self, action: #selector(ViewController.onClickBarButton(sender:)))
        myUIBarButtonRed.tag = 3

        // ボタンをツールバーに入れる.
        myToolbar.items = [myUIBarButtonGreen, myUIBarButtonBlue, myUIBarButtonRed]

        // ツールバーに追加する.
        self.view.addSubview(myToolbar)
    }

    /*
     UIBarButtonItemが押された際に呼ばれる.
     */
    @objc internal func onClickBarButton(sender: UIBarButtonItem) {

        switch sender.tag {
        case 1:
            self.view.backgroundColor = UIColor.green
        case 2:
            self.view.backgroundColor = UIColor.blue
        case 3:
            self.view.backgroundColor = UIColor.red
        default:
            print("ERROR!!")
        }
    }

    override func didReceiveMemoryWarning() {
        super.didReceiveMemoryWarning()
    }
}

```

```swift fct_label="Swift 3.x"
//
//  ViewController.swift
//  UIKit027_3.0
//
//  Created by KimikoWatanabe on 2016/08/13.
//  Copyright © 2016年 FaBo, Inc. All rights reserved.
//

import UIKit

class ViewController: UIViewController, UIToolbarDelegate{

    private var myToolbar: UIToolbar!

    override func viewDidLoad() {
        super.viewDidLoad()

        // 背景を青色に変更する.
        self.view.backgroundColor = UIColor.cyan

        // ツールバーのサイズを決める.
        myToolbar = UIToolbar(frame: CGRect(x:0, y:self.view.bounds.size.height - 44, width:self.view.bounds.size.width, height:40.0))

        // ツールバーの位置を決める.
        myToolbar.layer.position = CGPoint(x: self.view.bounds.width/2, y: self.view.bounds.height-20.0)

        // ツールバーの色を決める.
        myToolbar.barStyle = .blackTranslucent
        myToolbar.tintColor = UIColor.white
        myToolbar.backgroundColor = UIColor.black

        // ボタン１を生成する.
        let myUIBarButtonGreen: UIBarButtonItem = UIBarButtonItem(title: "Green", style:.plain, target: self, action: #selector(ViewController.onClickBarButton(sender:)))
        myUIBarButtonGreen.tag = 1

        // ボタン２を生成する.
        let myUIBarButtonBlue: UIBarButtonItem = UIBarButtonItem(title: "Blue", style:.plain, target: self, action: #selector(ViewController.onClickBarButton(sender:)))
        myUIBarButtonBlue.tag = 2

        // ボタン3を生成する.
        let myUIBarButtonRed: UIBarButtonItem = UIBarButtonItem(title: "Red", style:.plain, target: self, action: #selector(ViewController.onClickBarButton(sender:)))
        myUIBarButtonRed.tag = 3

        // ボタンをツールバーに入れる.
        myToolbar.items = [myUIBarButtonGreen, myUIBarButtonBlue, myUIBarButtonRed]

        // ツールバーに追加する.
        self.view.addSubview(myToolbar)
    }

    /*
     UIBarButtonItemが押された際に呼ばれる.
     */
    internal func onClickBarButton(sender: UIBarButtonItem) {

        switch sender.tag {
        case 1:
            self.view.backgroundColor = UIColor.green
        case 2:
            self.view.backgroundColor = UIColor.blue
        case 3:
            self.view.backgroundColor = UIColor.red
        default:
            print("ERROR!!")
        }
    }

    override func didReceiveMemoryWarning() {
        super.didReceiveMemoryWarning()
    }
}

```

```swift fct_label="Swift 2.3"
//
//  ViewController.swift
//  UIKit027_2.3
//
//  Created by KimikoWatanabe on 2016/08/16.
//  Copyright © 2016年 FaBo, Inc. All rights reserved.
//

import UIKit

class ViewController: UIViewController, UIToolbarDelegate{

    private var myToolbar: UIToolbar!

    override func viewDidLoad() {
        super.viewDidLoad()

        // 背景を青色に変更する.
        self.view.backgroundColor = UIColor.cyanColor()

        // ツールバーのサイズを決める.
        myToolbar = UIToolbar(frame: CGRectMake(0, self.view.bounds.size.height - 44, self.view.bounds.size.width, 40.0))

        // ツールバーの位置を決める.
        myToolbar.layer.position = CGPoint(x: self.view.bounds.width/2, y: self.view.bounds.height-20.0)

        // ツールバーの色を決める.
        myToolbar.barStyle = .BlackTranslucent
        myToolbar.tintColor = UIColor.whiteColor()
        myToolbar.backgroundColor = UIColor.blackColor()

        // ボタン１を生成する.
        let myUIBarButtonGreen: UIBarButtonItem = UIBarButtonItem(title: "Green", style:.Plain, target: self, action: #selector(ViewController.onClickBarButton(_:)))
        myUIBarButtonGreen.tag = 1

        // ボタン２を生成する.
        let myUIBarButtonBlue: UIBarButtonItem = UIBarButtonItem(title: "Blue", style:.Plain, target: self, action: #selector(ViewController.onClickBarButton(_:)))
        myUIBarButtonBlue.tag = 2

        // ボタン3を生成する.
        let myUIBarButtonRed: UIBarButtonItem = UIBarButtonItem(title: "Red", style:.Plain, target: self, action: #selector(ViewController.onClickBarButton(_:)))
        myUIBarButtonRed.tag = 3

        // ボタンをツールバーに入れる.
        myToolbar.items = [myUIBarButtonGreen, myUIBarButtonBlue, myUIBarButtonRed]

        // ツールバーに追加する.
        self.view.addSubview(myToolbar)
    }

    /*
     UIBarButtonItemが押された際に呼ばれる.
     */
    internal func onClickBarButton(sender: UIBarButtonItem) {

        switch sender.tag {
        case 1:
            self.view.backgroundColor = UIColor.greenColor()
        case 2:
            self.view.backgroundColor = UIColor.blueColor()
        case 3:
            self.view.backgroundColor = UIColor.redColor()
        default:
            print("ERROR!!")
        }
    }

    override func didReceiveMemoryWarning() {
        super.didReceiveMemoryWarning()
    }
}



```

## 3.0と4.0の差分
* ```internal func onClickBarButton(sender: UIBarButtonItem)``` に ```@objc``` を追加

## 2.3と3.0の差分
* UIColorの参照方法が変更(UIColor.grayColor()->UIColor.gray)
* CGRect,CGPointの初期化方法の変更(CGRectMake,CGPointMakeの廃止)

## Reference
* UIToolBar Class
 * [https://developer.apple.com/reference/uikit/uitoolbar](https://developer.apple.com/reference/uikit/uitoolbar)
