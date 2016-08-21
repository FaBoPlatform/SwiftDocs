# カスタマイズButton

![Preview uikit049](img/uikit049.png)

## Swift3.0
```swift
//
//  ViewController.swift
//  UIKit049_3.0
//
//  Created by KimikoWatanabe on 2016/08/21.
//  Copyright © 2016年 FaBo, Inc. All rights reserved.
//


```
### MyButton.swift
```swift
//
//  MyButton.swift
//  UIKit049_3.0
//
//  Created by KimikoWatanabe on 2016/08/21.
//  Copyright © 2016年 FaBo, Inc. All rights reserved.
//
```

## Swift 2.3
### ViewController.swift
```swift
//
//  ViewController.swift
//  UIKit049_2.3
//
//  Created by KimikoWatanabe on 2016/08/21.
//  Copyright © 2016年 FaBo, Inc. All rights reserved.
//

import UIKit

class ViewController: UIViewController {

    override func viewDidLoad() {
        super.viewDidLoad()

        let myButton = MyButton(frame: CGRectMake(50, 50, 100, 100))
        myButton.setTitle("ボタン", forState: UIControlState.Normal)
        myButton.setTitleColor(UIColor.blackColor(), forState: UIControlState.Normal)
        myButton.addTarget(self, action: #selector(ViewController.onClickMyButton(_:)), forControlEvents: .TouchUpInside)

        self.view.addSubview(myButton)

    }

    override func didReceiveMemoryWarning() {
        super.didReceiveMemoryWarning()
        // Dispose of any resources that can be recreated.
    }

    /*
     ボタンイベント.
     */
    func onClickMyButton(sender: UIButton){
        print("onClickMyButton:")
        print("sender.currentTitile: \(sender.currentTitle!)")
    }

}
```

### MyButton.swift
```swift
//
//  MyButton.swift
//  UIKit049_2.3
//
//  Created by KimikoWatanabe on 2016/08/21.
//  Copyright © 2016年 FaBo, Inc. All rights reserved.
//

import Foundation
import UIKit

class MyButton: UIButton {

    var myStatus: ButtonStatus!
    enum ButtonStatus {
        case Normal
        case TouchBegan
        case TouchEnded
    }

    required init(coder aDecoder: NSCoder) {
        super.init(coder: aDecoder)!
    }

    override init(frame: CGRect) {
        super.init(frame: frame)
        myStatus = .Normal
    }
    override func touchesBegan(touches: Set<UITouch>, withEvent event: UIEvent?) {
        super.touchesBegan(touches, withEvent: event)

        myStatus = .TouchBegan
        self.setNeedsDisplay()
    }

    override func touchesEnded(touches: Set<UITouch>, withEvent event: UIEvent?) {
        super.touchesEnded(touches, withEvent: event)

        myStatus = .TouchEnded
        self.setNeedsDisplay()
    }

    override func drawRect(rect: CGRect) {

        let width = rect.width, height = rect.height

        if myStatus == .Normal || myStatus == .TouchEnded {
            // 色の定義.
            let color = UIColor(red: 0.081, green: 1.000, blue: 0.421, alpha: 1.000)

            // ボタンの形状を定義.
            let path = makeNormalPath(width, height: height)
            color.setFill()
            path.fill()
            path.lineWidth = 0
            path.stroke()

        } else if myStatus == .TouchBegan {
            // 色の定義.
            let color = UIColor(red: 1.000, green: 1.000, blue: 0.421, alpha: 1.000)

            // ボタンの形状を定義.
            let path = makeNormalPath(width, height: height)
            color.setFill()
            path.fill()
            path.lineWidth = 0
            path.stroke()
        }

        super.drawRect(rect)
    }

    /*
     ボタンの形状を作成.
     */
    private func makeNormalPath(width: CGFloat, height: CGFloat) -> UIBezierPath{
        let bezierPath = UIBezierPath()
        bezierPath.moveToPoint(CGPointMake(46.33/120 * width, 0.5/120 * height))
        bezierPath.addCurveToPoint(CGPointMake(37.47/120 * width, 30.41/120 * height),
                                   controlPoint1: CGPointMake(38.21/120 * width, 0.5/120 * height),
                                   controlPoint2: CGPointMake(45.96/120 * width, 18.21/120 * height))
        bezierPath.addCurveToPoint(CGPointMake(0.54/120 * width, 30.41/120 * height),
                                   controlPoint1: CGPointMake(28.97/120 * width, 42.61/120 * height),
                                   controlPoint2: CGPointMake(2.75/120 * width, 21.75/120 * height))
        bezierPath.addCurveToPoint(CGPointMake(28.61/120 * width, 65.04/120 * height),
                                   controlPoint1: CGPointMake(-1.68/120 * width, 39.06/120 * height),
                                   controlPoint2: CGPointMake(28.61/120 * width, 53.23/120 * height))
        bezierPath.addCurveToPoint(CGPointMake(12.36/120 * width, 96.52/120 * height),
                                   controlPoint1: CGPointMake(28.61/120 * width, 76.84/120 * height),
                                   controlPoint2: CGPointMake(4.23/120 * width, 91.79/120 * height))
        bezierPath.addCurveToPoint(CGPointMake(61.1/120 * width, 83.92/120 * height),
                                   controlPoint1: CGPointMake(20.48/120 * width, 101.24/120 * height),
                                   controlPoint2: CGPointMake(38.21/120 * width, 83.92/120 * height))
        bezierPath.addCurveToPoint(CGPointMake(100.99/120 * width, 105.96/120 * height),
                                   controlPoint1: CGPointMake(84/120 * width, 83.92/120 * height),
                                   controlPoint2: CGPointMake(95.82/120 * width, 114.62/120 * height))
        bezierPath.addCurveToPoint(CGPointMake(86.22/120 * width, 65.04/120 * height),
                                   controlPoint1: CGPointMake(106.16/120 * width, 97.3/120 * height),
                                   controlPoint2: CGPointMake(83.63/120 * width, 81.56/120 * height))
        bezierPath.addCurveToPoint(CGPointMake(114.29/120 * width, 13.09/120 * height),
                                   controlPoint1: CGPointMake(88.8/120 * width, 48.51/120 * height),
                                   controlPoint2: CGPointMake(117.24/120 * width, 17.81/120 * height))
        bezierPath.addCurveToPoint(CGPointMake(69.97/120 * width, 30.41/120 * height),
                                   controlPoint1: CGPointMake(111.33/120 * width, 8.37/120 * height),
                                   controlPoint2: CGPointMake(86.96/120 * width, 37.88/120 * height))
        bezierPath.addCurveToPoint(CGPointMake(46.33/120 * width, 0.5/120 * height),
                                   controlPoint1: CGPointMake(52.98/120 * width, 22.93/120 * height),
                                   controlPoint2: CGPointMake(54.46/120 * width, 0.5/120 * height))
        bezierPath.closePath()

        return bezierPath
    }
}

```

## 2.3と3.0の差分
* UIColorの参照方法が変更(UIColor.grayColor()->UIColor.gray)
* CGRect,CGPointの初期化方法の変更(CGRectMake,CGPointMakeの廃止)

## Reference
* UIButton Class
