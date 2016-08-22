# タイマーを止める

![Preview uikit0]()

## Swift3.0
```swift
//
//  ViewController.swift
//  Utility000_3.0
//
//  Created by KimikoWatanabe on 2016/08/22.
//  Copyright © 2016年 FaBo, Inc. All rights reserved.
//


```

## Swift 2.3
```swift
//
//  ViewController.swift
//  Utility000_2.3
//
//  Created by KimikoWatanabe on 2016/08/22.
//  Copyright © 2016年 FaBo, Inc. All rights reserved.
//

import UIKit

class ViewController: UIViewController {

    //時間計測用の変数.
    var cnt : Float = 0

    //時間表示用のラベル.
    var myLabel : UILabel!

    //タイマー.
    var timer : NSTimer!

    override func viewDidLoad() {

        //ラベルを作る.
        myLabel = UILabel(frame: CGRectMake(0,0,200,50))
        myLabel.backgroundColor = UIColor.orangeColor()
        myLabel.layer.masksToBounds = true
        myLabel.layer.cornerRadius = 20.0
        myLabel.text = "Time:\(cnt)"
        myLabel.textColor = UIColor.whiteColor()
        myLabel.shadowColor = UIColor.grayColor()
        myLabel.textAlignment = NSTextAlignment.Center
        myLabel.layer.position = CGPoint(x: self.view.bounds.width/2,y: 200)
        self.view.backgroundColor = UIColor.cyanColor()
        self.view.addSubview(myLabel)

        //タイマー停止ボタンを作る.
        let myButton = UIButton(frame: CGRectMake(0, 0, 200, 50))
        myButton.layer.masksToBounds = true
        myButton.layer.cornerRadius = 20.0
        myButton.backgroundColor = UIColor.blueColor()
        myButton.setTitle("Stop Timer", forState: UIControlState.Normal)
        myButton.layer.position = CGPointMake(self.view.center.x, self.view.center.y + 100)
        myButton.addTarget(self, action: #selector(ViewController.onMyButtonClick(_:)), forControlEvents: UIControlEvents.TouchUpInside)
        self.view.addSubview(myButton)

        //タイマーを作る.
        timer = NSTimer.scheduledTimerWithTimeInterval(0.1, target: self, selector: #selector(ViewController.onUpdate(_:)), userInfo: nil, repeats: true)
    }

    //ボタンが押された時に呼ばれるメソッド.
    func onMyButtonClick(sender : UIButton){

        //timerが動いてるなら.
        if timer.valid == true {

            //timerを破棄する.
            timer.invalidate()

            //ボタンのタイトル変更.
            sender.setTitle("Start Timer", forState: UIControlState.Normal)
        }
        else{

            //timerを生成する.
            timer = NSTimer.scheduledTimerWithTimeInterval(0.1, target: self, selector: #selector(ViewController.onUpdate(_:)), userInfo: nil, repeats: true)

            //ボタンのタイトル変更.
            sender.setTitle("Stop Timer", forState: UIControlState.Normal)
        }

    }

    //NSTimerIntervalで指定された秒数毎に呼び出されるメソッド.
    func onUpdate(timer : NSTimer){

        cnt += 0.1

        //桁数を指定して文字列を作る.
        let str = "Time:".stringByAppendingFormat("%.1f",cnt)

        myLabel.text = str

    }

}

```

## 2.3と3.0の差分
* UIColorの参照方法が変更(UIColor.grayColor()->UIColor.gray)
* CGRect,CGPointの初期化方法の変更(CGRectMake,CGPointMakeの廃止)

## Reference
* invalidate Tasks
