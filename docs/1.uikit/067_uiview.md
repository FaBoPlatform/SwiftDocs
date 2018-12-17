# UIViewをスナップさせる

![Preview uikit067](img/uikit067.png)

## Swift3.0
```swift
//
//  ViewController.swift
//  UIKit067
//
//  Created by Misato Morino on 2016/08/15.
//  Copyright © 2016年 Misato Morino. All rights reserved.
//

import UIKit

class ViewController: UIViewController {
    
    // UIDynamicAnimatorのインスタンスを保存しなければアニメーションが実行されない.
    var animator : UIDynamicAnimator!
    
    var myLabel : UILabel!
    
    override func viewDidLoad() {
        
        super.viewDidLoad()
        
        // 背景を水色に設定.
        self.view.backgroundColor = UIColor.cyan
        
        // Labelを作成.
        myLabel = UILabel(frame: CGRect(x: 0, y: 0, width: 200, height: 50))
        myLabel.backgroundColor = UIColor.orange
        myLabel.layer.masksToBounds = true
        myLabel.layer.cornerRadius = 20.0
        myLabel.text = "Hello Swift!!"
        myLabel.textColor = UIColor.white
        myLabel.shadowColor = UIColor.gray
        myLabel.textAlignment = NSTextAlignment.center
        myLabel.layer.position = CGPoint(x: self.view.bounds.width/2,y: 200)
        self.view.backgroundColor = UIColor.cyan
        self.view.addSubview(myLabel)
        
        // UIDynamiAnimatorの生成とインスタンスの保存.
        animator = UIDynamicAnimator(referenceView: self.view)
    }
    
    /*
     タップを感知した時に呼ばれるメソッド.
     */
    override func touchesBegan(_ touches: Set<UITouch>, with event: UIEvent?) {
           
        for touch : AnyObject in touches {
            
            // タッチされた座標を取得.
            let location = touch.location(in: self.view)
            
            // animatorに登録されていたBahaviorを全て削除.
            animator.removeAllBehaviors()
            
            // UIViewをスナップさせるUISnapBehaviorを生成.
            let snap = UISnapBehavior(item: myLabel, snapTo: location)
            
            // スナップを適用させる.
            animator.addBehavior(snap)
        }
    }
} 
```

## Swift 2.3
```swift
//
//  ViewController.swift
//  UIKit067
//
//  Created by Misato Morino on 2016/08/15.
//  Copyright © 2016年 Misato Morino. All rights reserved.
//

import UIKit

class ViewController: UIViewController {
    
    // UIDynamicAnimatorのインスタンスを保存しなければアニメーションが実行されない.
    var animator : UIDynamicAnimator!
    
    var myLabel : UILabel!
    
    override func viewDidLoad() {
        
        super.viewDidLoad()
        
        // 背景を水色に設定.
        self.view.backgroundColor = UIColor.cyanColor()
        
        // Labelを作成.
        myLabel = UILabel(frame: CGRectMake(0,0,200,50))
        myLabel.backgroundColor = UIColor.orangeColor()
        myLabel.layer.masksToBounds = true
        myLabel.layer.cornerRadius = 20.0
        myLabel.text = "Hello Swift!!"
        myLabel.textColor = UIColor.whiteColor()
        myLabel.shadowColor = UIColor.grayColor()
        myLabel.textAlignment = NSTextAlignment.Center
        myLabel.layer.position = CGPoint(x: self.view.bounds.width/2,y: 200)
        self.view.backgroundColor = UIColor.cyanColor()
        self.view.addSubview(myLabel)
        
        // UIDynamiAnimatorの生成とインスタンスの保存.
        animator = UIDynamicAnimator(referenceView: self.view)
    }
    
    /*
     タップを感知した時に呼ばれるメソッド.
     */
    override func touchesBegan(touches: Set<UITouch>, withEvent event: UIEvent?) {
        
        for touch : AnyObject in touches {
            
            // タッチされた座標を取得.
            let location = touch.locationInView(self.view)
            
            // animatorに登録されていたBahaviorを全て削除.
            animator.removeAllBehaviors()
            
            // UIViewをスナップさせるUISnapBehaviorを生成.
            let snap = UISnapBehavior(item: myLabel, snapToPoint: location)
            
            // スナップを適用させる.
            animator.addBehavior(snap)
        }
    }
}
```

## 2.3と3.0の差分

* ```touch.locationInView(self.view)``` から ```touch.location(in: self.view)``` に変更
* ```UISnapBehavior(item: myLabel, snapToPoint: location)``` から ```UISnapBehavior(item: myLabel, snapTo: location)``` に変更

## Reference

* UIDynamicAnimator
    * [https://developer.apple.com/reference/uikit/uidynamianimator](https://developer.apple.com/reference/uikit/uidynamianimator)
* UISnapBehavior
    * [https://developer.apple.com/reference/uikit/uisnapbehavior](https://developer.apple.com/reference/uikit/uisnapbehavior)
* UIView
    * [https://developer.apple.com/reference/uikit/uipushbehavioe](https://developer.apple.com/reference/uikit/uipushBehavioe)
