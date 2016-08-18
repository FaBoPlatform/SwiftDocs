# UIViewにトランジションをつける

![Preview uikit036_001](img/uikit036_001.png)
![Preview uikit036_002](img/uikit036_002.png)

## Swift3.0
```swift
//
//  ViewController.swift
//  UIKit036_3.0
//
//  Created by KimikoWatanabe on 2016/08/18.
//  Copyright © 2016年 FaBo, Inc. All rights reserved.
//


```

## Swift 2.3
```swift
//
//  ViewController.swift
//  UIKit036_2.3
//
//  Created by KimikoWatanabe on 2016/08/18.
//  Copyright © 2016年 FaBo, Inc. All rights reserved.
//

import UIKit

class ViewController: UIViewController {

    private var myView: UIView!
    private var myButton: UIButton!

    // View切り替え用フラグ.
    var viewFlag = true

    override func viewDidLoad() {
        super.viewDidLoad()

        // 背景を青色に設定.
        self.view.backgroundColor = UIColor.cyanColor()

        // myViewを生成.
        myView = UIView(frame: self.view.frame)
        myView.backgroundColor = UIColor.orangeColor()
        myView.hidden = true
        myView.layer.position = CGPointMake(self.view.frame.width/2, self.view.frame.height/2)

        // ボタンを生成.
        myButton = UIButton(frame: CGRectMake(0, 0, 100, 50))
        myButton.backgroundColor = UIColor.redColor()
        myButton.layer.masksToBounds = true
        myButton.layer.cornerRadius = 20.0
        myButton.setTitle("Transition", forState: .Normal)
        myButton.setTitleColor(UIColor.whiteColor(), forState: .Normal)
        myButton.layer.position = CGPointMake(self.view.frame.width/2, self.view.frame.height/2)
        myButton.addTarget(self, action: #selector(ViewController.onClickMyButton(_:)), forControlEvents: .TouchUpInside)

        // self.viewにmyViewを追加.
        self.view.addSubview(myView)

        // self.viewにボタンを追加.
        self.view.addSubview(myButton)
    }

    /*
     ボタンイベント
     */
    internal func onClickMyButton(sender: UIButton) {

        // フラグがtrue.
        if viewFlag {

            // アニメーション処理.
            // どのviewからtransitionするか.
            UIView.transitionFromView(self.view,

                                      // transition先のview.
                toView: myView,

                // 秒数(1秒).
                duration: 1.0,

                // transitionを設定.
                options: UIViewAnimationOptions.TransitionCurlUp,

                // アニメーション完了時の処理.
                completion: { (Bool) -> Void in

                    print("self.view -> myView")

                    // myViewを表示.
                    self.myView.hidden = false

                    // myViewにボタンを追加.
                    self.myView.addSubview(self.myButton)
            })
            // フラグを折る.
            viewFlag = false

        } else {
            UIView.transitionFromView(myView,
                                      toView: self.view,
                                      duration: 1.0,
                                      options: UIViewAnimationOptions.TransitionFlipFromLeft,
                                      completion: { (Bool) -> Void in

                                        print("myView -> self.view")

                                        // self.viewにボタンを追加.
                                        self.view.addSubview(self.myButton)
            })
            // フラグを立てる.
            viewFlag = true
        }
    }

    override func didReceiveMemoryWarning() {
        super.didReceiveMemoryWarning()
    }
}
```

## 2.3と3.0の差分
* UIColorの参照方法が変更(UIColor.grayColor()->UIColor.gray)
* CGRect,CGPointの初期化方法の変更(CGRectMake,CGPointMakeの廃止)

## Reference
* UIView Class
