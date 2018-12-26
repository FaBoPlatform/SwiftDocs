# UIAlertControllerで文字数制限を設ける

![Preview uikit042](img/uikit042.png)

```swift fct_label="Swift 4.x"
//
//  ViewController.swift
//  UIKit042_4.0
//
//  Created by KimikoWatanabe on 2016/08/21.
//  Copyright © 2016年 FaBo, Inc. All rights reserved.
//

import UIKit

class ViewController: UIViewController {
    
    var InputStr:String!
    
    // 入力できる最大文字数.
    let maxLength: Int = 6
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        self.view.backgroundColor = UIColor.cyan
        
        let myButton = UIButton(frame: CGRect(x:0, y:0, width:200, height:40))
        myButton.layer.position = CGPoint(x:self.view.frame.width/2, y:200)
        myButton.layer.cornerRadius = 20.0
        myButton.backgroundColor = UIColor.red
        myButton.setTitle("UIAlertを発動", for: .normal)
        myButton.addTarget(self, action: #selector(ViewController.onClickMyButton(sender:)), for: .touchUpInside)
        
        self.view.addSubview(myButton)
    }
    
    override func didReceiveMemoryWarning() {
        super.didReceiveMemoryWarning()
    }
    
    @objc func onClickMyButton(sender: UIButton) {
        
        // Alert生成.
        let myAlert: UIAlertController = UIAlertController(title: "title", message: "6文字以下で入力", preferredStyle: UIAlertController.Style.alert)
        
        
        // OKアクション生成.
        let OkAction = UIAlertAction(title: "OK", style: UIAlertAction.Style.default) { (action: UIAlertAction!) -> Void in
            print("OK")
        }
        
        // Cancelアクション生成.
        let CancelAction = UIAlertAction(title: "Cancel", style: UIAlertAction.Style.destructive) { (action: UIAlertAction!) -> Void in
            print("Cancel")
        }
        
        // AlertにTextFieldを追加.
        myAlert.addTextField { (textField: UITextField!) -> Void in
            
            // NotificationCenterを生成.
            let myNotificationCenter = NotificationCenter.default
            
            // textFieldに変更があればchangeTextFieldメソッドに通知.
            myNotificationCenter.addObserver(self, selector: #selector(ViewController.changeTextField(sender:)), name: UITextField.textDidChangeNotification, object: nil)
        }
        
        // Alertにアクションを追加.
        myAlert.addAction(OkAction)
        myAlert.addAction(CancelAction)
        
        // Alertを発動する.
        present(myAlert, animated: true, completion: nil)
    }
    
    @objc func changeTextField (sender: NSNotification) {
        
        let textField = sender.object as! UITextField
        
        // 入力された文字を取得.
        InputStr = textField.text
        
        // 入力された文字が6文字を超えたら入力を制限.
        if InputStr.characters.count < maxLength {
            textField.isEnabled = true
            
            // 入力された文字を表示.
            print(textField.text!)
        } else {
            textField.isEnabled = false
            print("6文字を超えています")
        }
    }
}
```

```swift fct_label="Swift 3.x"
//
//  ViewController.swift
//  UIKit042_3.0
//
//  Created by KimikoWatanabe on 2016/08/21.
//  Copyright © 2016年 FaBo, Inc. All rights reserved.
//

import UIKit

class ViewController: UIViewController {

    var InputStr:String!

    // 入力できる最大文字数.
    let maxLength: Int = 6

    override func viewDidLoad() {
        super.viewDidLoad()

        self.view.backgroundColor = UIColor.cyan

        let myButton = UIButton(frame: CGRect(x:0, y:0, width:200, height:40))
        myButton.layer.position = CGPoint(x:self.view.frame.width/2, y:200)
        myButton.layer.cornerRadius = 20.0
        myButton.backgroundColor = UIColor.red
        myButton.setTitle("UIAlertを発動", for: .normal)
        myButton.addTarget(self, action: #selector(ViewController.onClickMyButton(sender:)), for: .touchUpInside)

        self.view.addSubview(myButton)
    }

    override func didReceiveMemoryWarning() {
        super.didReceiveMemoryWarning()
    }

    func onClickMyButton(sender: UIButton) {

        // Alert生成.
        let myAlert: UIAlertController = UIAlertController(title: "title", message: "6文字以下で入力", preferredStyle: UIAlertControllerStyle.alert)


        // OKアクション生成.
        let OkAction = UIAlertAction(title: "OK", style: UIAlertActionStyle.default) { (action: UIAlertAction!) -> Void in
            print("OK")
        }

        // Cancelアクション生成.
        let CancelAction = UIAlertAction(title: "Cancel", style: UIAlertActionStyle.destructive) { (action: UIAlertAction!) -> Void in
            print("Cancel")
        }

        // AlertにTextFieldを追加.
        myAlert.addTextField { (textField: UITextField!) -> Void in

            // NotificationCenterを生成.
            let myNotificationCenter = NotificationCenter.default

            // textFieldに変更があればchangeTextFieldメソッドに通知.
            myNotificationCenter.addObserver(self, selector: #selector(ViewController.changeTextField(sender:)), name: NSNotification.Name.UITextFieldTextDidChange, object: nil)
        }

        // Alertにアクションを追加.
        myAlert.addAction(OkAction)
        myAlert.addAction(CancelAction)

        // Alertを発動する.
        present(myAlert, animated: true, completion: nil)
    }

    func changeTextField (sender: NSNotification) {

        let textField = sender.object as! UITextField

        // 入力された文字を取得.
        InputStr = textField.text

        // 入力された文字が6文字を超えたら入力を制限.
        if InputStr.characters.count < maxLength {
            textField.isEnabled = true

            // 入力された文字を表示.
            print(textField.text!)
        } else {
            textField.isEnabled = false
            print("6文字を超えています")
        }
    }
}
```

```swift fct_label="Swift 2.3"
//
//  ViewController.swift
//  UIKit042_2.3
//
//  Created by KimikoWatanabe on 2016/08/21.
//  Copyright © 2016年 FaBo, Inc. All rights reserved.
//

import UIKit

class ViewController: UIViewController {

    var InputStr:String!

    // 入力できる最大文字数.
    let maxLength: Int = 6

    override func viewDidLoad() {
        super.viewDidLoad()

        self.view.backgroundColor = UIColor.cyanColor()

        let myButton = UIButton(frame: CGRectMake(0, 0, 200, 40))
        myButton.layer.position = CGPointMake(self.view.frame.width/2, 200)
        myButton.layer.cornerRadius = 20.0
        myButton.backgroundColor = UIColor.redColor()
        myButton.setTitle("UIAlertを発動", forState: .Normal)
        myButton.addTarget(self, action: #selector(ViewController.onClickMyButton(_:)), forControlEvents: .TouchUpInside)

        self.view.addSubview(myButton)
    }

    override func didReceiveMemoryWarning() {
        super.didReceiveMemoryWarning()
    }

    func onClickMyButton(sender: UIButton) {

        // Alert生成.
        let myAlert: UIAlertController = UIAlertController(title: "title", message: "6文字以下で入力", preferredStyle: UIAlertControllerStyle.Alert)


        // OKアクション生成.
        let OkAction = UIAlertAction(title: "OK", style: UIAlertActionStyle.Default) { (action: UIAlertAction!) -> Void in
            print("OK")
        }

        // Cancelアクション生成.
        let CancelAction = UIAlertAction(title: "Cancel", style: UIAlertActionStyle.Destructive) { (action: UIAlertAction!) -> Void in
            print("Cancel")
        }

        // AlertにTextFieldを追加.
        myAlert.addTextFieldWithConfigurationHandler { (textField: UITextField!) -> Void in

            // NotificationCenterを生成.
            let myNotificationCenter = NSNotificationCenter.defaultCenter()

            // textFieldに変更があればchangeTextFieldメソッドに通知.
            myNotificationCenter.addObserver(self, selector: #selector(ViewController.changeTextField(_:)), name: UITextFieldTextDidChangeNotification, object: nil)
        }

        // Alertにアクションを追加.
        myAlert.addAction(OkAction)
        myAlert.addAction(CancelAction)

        // Alertを発動する.
        presentViewController(myAlert, animated: true, completion: nil)
    }

    func changeTextField (sender: NSNotification) {

        let textField = sender.object as! UITextField

        // 入力された文字を取得.
        InputStr = textField.text

        // 入力された文字が6文字を超えたら入力を制限.
        if InputStr.characters.count < maxLength {
            textField.enabled = true

            // 入力された文字を表示.
            print(textField.text!)
        } else {
            textField.enabled = false
            print("6文字を超えています")
        }
    }
}
```

## 3.xと4.xの差分
* ```onClickMyButton``` 、 ```changeTextField``` に ```@objc``` を追加
* ```NSNotification.Name.UITextFieldTextDidChange``` が ```UITextField.textDidChangeNotification``` に変更
* ```UIAlertControllerStyle``` が ```UIAlertController.Style``` に変更
* ```characters``` は非推奨

## 2.3と3.0の差分
* UIColorの参照方法が変更(UIColor.grayColor()->UIColor.gray)
* CGRect,CGPointの初期化方法の変更(CGRectMake,CGPointMakeの廃止)
* senderの記述の変更(onClickMyButton(_:)->onClickMyButton(sender:)

## Reference
* UIAlertController Class
 * [https://developer.apple.com/reference/uikit/uialertcontroller](https://developer.apple.com/reference/uikit/uialertcontroller)
* UIAlertAction Class
 * [https://developer.apple.com/reference/uikit/uialertaction](https://developer.apple.com/reference/uikit/uialertaction)
