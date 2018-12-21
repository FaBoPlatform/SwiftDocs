# プルンプルンするButtonの作成

![Preview uikit048](img/uikit048.png)

```swift fct_label="Swift 4.x/Swift 3.x"
//
//  ViewController.swift
//  UIKit048_4.0
//
//  Created by KimikoWatanabe on 2016/08/21.
//  Copyright © 2016年 FaBo, Inc. All rights reserved.
//

import UIKit

class ViewController: UIViewController {

    // ボタン.
    var myButton: UIButton!

    override func viewDidLoad() {
        super.viewDidLoad()

        // ボタンを作成する.
        myButton = UIButton()
        myButton.frame = CGRect(x:0,y:0,width:100,height:100)
        myButton.backgroundColor = UIColor.green
        myButton.layer.masksToBounds = true
        myButton.setTitle("ボタン", for: UIControlState.normal)
        myButton.setTitleColor(UIColor.white, for: UIControlState.normal)
        myButton.layer.cornerRadius = 50.0
        myButton.layer.position = CGPoint(x: self.view.frame.width/2, y:self.view.frame.height/2)

        // TouchDownの時のイベントを追加する.
        myButton.addTarget(self, action: #selector(ViewController.onDownButton(sender:)), for: .touchDown)

        // TouchUpの時のイベントを追加する.
        myButton.addTarget(self, action: #selector(ViewController.onUpButton(sender:)), for: [.touchUpInside,.touchUpOutside])

        // 背景色を黒に設定する.
        self.view.backgroundColor = UIColor.black

        // ボタンをViewに追加する.
        self.view.addSubview(myButton);
    }

    /*
     ボタンイベント(Down)
     */
    func onDownButton(sender: UIButton){
        //UIView.animateWithDuration
        UIView.animate(withDuration: 0.06,

                                   // アニメーション中の処理.
            animations: { () -> Void in

                // 縮小用アフィン行列を作成する.
                self.myButton.transform = CGAffineTransform(scaleX: 0.9, y: 0.9)

            })
        { (Bool) -> Void in

        }
    }

    /*
     ボタンイベント(Up)
     */
    func onUpButton(sender: UIButton){
        UIView.animate(withDuration: 0.1,

                                   // アニメーション中の処理.
            animations: { () -> Void in

                // 拡大用アフィン行列を作成する.
                self.myButton.transform = CGAffineTransform(scaleX: 0.4, y: 0.4)

                // 縮小用アフィン行列を作成する.
                self.myButton.transform = CGAffineTransform(scaleX: 1.0, y: 1.0)

            })
        { (Bool) -> Void in

        }
    }

    override func didReceiveMemoryWarning() {
        super.didReceiveMemoryWarning()
        // Dispose of any resources that can be recreated.
    }
}
```

```swift fct_label="Swift 2.3"
//
//  ViewController.swift
//  UIKit048_2.3
//
//  Created by KimikoWatanabe on 2016/08/21.
//  Copyright © 2016年 FaBo, Inc. All rights reserved.
//

import UIKit

class ViewController: UIViewController {

    // ボタン.
    var myButton: UIButton!

    override func viewDidLoad() {
        super.viewDidLoad()

        // ボタンを作成する.
        myButton = UIButton()
        myButton.frame = CGRectMake(0,0,100,100)
        myButton.backgroundColor = UIColor.greenColor();
        myButton.layer.masksToBounds = true
        myButton.setTitle("ボタン", forState: UIControlState.Normal)
        myButton.setTitleColor(UIColor.whiteColor(), forState: UIControlState.Normal)
        myButton.layer.cornerRadius = 50.0
        myButton.layer.position = CGPoint(x: self.view.frame.width/2, y:self.view.frame.height/2)

        // TouchDownの時のイベントを追加する.
        myButton.addTarget(self, action: #selector(ViewController.onDownButton(_:)), forControlEvents: .TouchDown)

        // TouchUpの時のイベントを追加する.
        myButton.addTarget(self, action: #selector(ViewController.onUpButton(_:)), forControlEvents: [.TouchUpInside,.TouchUpOutside])

        // 背景色を黒に設定する.
        self.view.backgroundColor = UIColor.blackColor()

        // ボタンをViewに追加する.
        self.view.addSubview(myButton);
    }

    /*
     ボタンイベント(Down)
     */
    func onDownButton(sender: UIButton){
        UIView.animateWithDuration(0.06,

                                   // アニメーション中の処理.
            animations: { () -> Void in

                // 縮小用アフィン行列を作成する.
                self.myButton.transform = CGAffineTransformMakeScale(0.9, 0.9)

            })
        { (Bool) -> Void in

        }
    }

    /*
     ボタンイベント(Up)
     */
    func onUpButton(sender: UIButton){
        UIView.animateWithDuration(0.1,

                                   // アニメーション中の処理.
            animations: { () -> Void in

                // 拡大用アフィン行列を作成する.
                self.myButton.transform = CGAffineTransformMakeScale(0.4, 0.4)

                // 縮小用アフィン行列を作成する.
                self.myButton.transform = CGAffineTransformMakeScale(1.0, 1.0)

            })
        { (Bool) -> Void in

        }
    }

    override func didReceiveMemoryWarning() {
        super.didReceiveMemoryWarning()
        // Dispose of any resources that can be recreated.
    }
}
```

## 2.3と3.0の差分
* UIColorの参照方法が変更(UIColor.grayColor()->UIColor.gray)
* CGRect,CGPointの初期化方法の変更(CGRectMake,CGPointMakeの廃止)
* UIView.animateWithDurationメソッドがUIView.animateに変更
* CGAffineTransformMakeScaleの廃止、CGAffineTransformに統合
## Reference
* UIButton Class
 * [https://developer.apple.com/reference/uikit/uibutton](https://developer.apple.com/reference/uikit/uibutton)
* CGAffinTransform Class
 * [https://developer.apple.com/reference/coregraphics/cgaffinetransform](https://developer.apple.com/reference/coregraphics/cgaffinetransform)
