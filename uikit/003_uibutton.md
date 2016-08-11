# UIButtonでボタンを表示

![Preview uikit002](./img/uikit002.png) ![Preview uikit002_1](./img/uikit002_1.png)

[![Banner](../img/fabo_banner.png)](http://www.fabo.io)

## Swift 3.0

```swift
//
//  ViewController.swift
//  UIKit002
//
//  Copyright © 2016年 FaBo, Inc. All rights reserved.
//

import UIKit

class ViewController: UIViewController {
    
    private var myButton: UIButton!
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        // Buttonを生成する.
        myButton = UIButton()
        
        // ボタンのサイズ.
        let bWidth: CGFloat = 200
        let bHeight: CGFloat = 50
        
        // ボタンのX,Y座標.
        let posX: CGFloat = self.view.frame.width/2 - bWidth/2
        let posY: CGFloat = self.view.frame.height/2 - bHeight/2
        
        // ボタンの設置座標とサイズを設定する.
        myButton.frame = CGRect(x: posX, y: posY, width: bWidth, height: bHeight)
        
        // ボタンの背景色を設定.
        myButton.backgroundColor = UIColor.red
        
        // ボタンの枠を丸くする.
        myButton.layer.masksToBounds = true
        
        // コーナーの半径を設定する.
        myButton.layer.cornerRadius = 20.0
        
        // タイトルを設定する(通常時).
        myButton.setTitle("ボタン(通常)", for: .normal)
        myButton.setTitleColor(UIColor.white, for: .normal)
        
        // タイトルを設定する(ボタンがハイライトされた時).
        myButton.setTitle("ボタン(押された時)", for: .highlighted)
        myButton.setTitleColor(UIColor.black, for: .highlighted)
        
        // ボタンにタグをつける.
        myButton.tag = 1
        
        // イベントを追加する
        myButton.addTarget(self, action: #selector(ViewController.onClickMyButton(sender:)), for: .touchUpInside)
        
        // ボタンをViewに追加.
        self.view.addSubview(myButton)
        
    }
    
    override func didReceiveMemoryWarning() {
        super.didReceiveMemoryWarning()
        // Dispose of any resources that can be recreated.
    }
    
    /*
     ボタンのイベント.
     */
    internal func onClickMyButton(sender: UIButton) {
        print("onClickMyButton:");
        print("sender.currentTitle: \(sender.currentTitle)")
        print("sender.tag: \(sender.tag)")
    }
    
}


```

## Swift 2.3

```swift
//
//  ViewController.swift
//  UIKit003
//
//  Copyright © 2016年 FaBo, Inc. All rights reserved.
//

import UIKit

class ViewController: UIViewController {

    private var myInfoDarkButton: UIButton!
    private var myInfoLightButton: UIButton!
    private var myAddButton: UIButton!
    private var myDetailButton: UIButton!
    private var mySystemButton: UIButton!
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        // ボタンを生成.
        myInfoDarkButton = UIButton(type: .InfoDark)
        myInfoLightButton = UIButton(type: .InfoLight)
        myAddButton = UIButton(type: .ContactAdd)
        myDetailButton = UIButton(type: .DetailDisclosure)
        mySystemButton = UIButton(type: .System)
        
        // ボタンの位置を指定する
        let posX: CGFloat = self.view.frame.width/2
        myInfoDarkButton.layer.position = CGPoint(x: posX, y: 50)
        myInfoLightButton.layer.position = CGPoint(x: posX, y: 100)
        myAddButton.layer.position = CGPoint(x: posX, y: 150)
        myDetailButton.layer.position = CGPoint(x: posX, y: 200)
        
        // Systemボタンのサイズ.
        let sWidth: CGFloat = 200
        let sHeight: CGFloat = 50
        // Systemボタンの配置するx,y座標
        let sposX: CGFloat = self.view.frame.width/2 - sWidth/2
        let sposY: CGFloat = 250
        
        // Systemボタンに配置するx,y座標とサイズを設定.
        mySystemButton.frame = CGRectMake(sposX, sposY, sWidth, sHeight)
        
        // Systemボタンにタイトルを設定する.
        mySystemButton.setTitle("MySystemButton", forState: .Normal)
        
        // タグを設定する.
        myInfoDarkButton.tag = 1
        myInfoLightButton.tag = 2
        myAddButton.tag = 3
        myDetailButton.tag = 4
        mySystemButton.tag = 5
        
        // イベントを追加する
        myInfoDarkButton.addTarget(self, action: #selector(ViewController.onClickMyButton(_:)), forControlEvents: .TouchDown)
        myInfoLightButton.addTarget(self, action: #selector(ViewController.onClickMyButton(_:)), forControlEvents: .TouchDown)
        myAddButton.addTarget(self, action: #selector(ViewController.onClickMyButton(_:)), forControlEvents: .TouchDown)
        myDetailButton.addTarget(self, action: #selector(ViewController.onClickMyButton(_:)), forControlEvents: .TouchDown)
        mySystemButton.addTarget(self, action: #selector(ViewController.onClickMyButton(_:)), forControlEvents: .TouchDown)
        
        // ボタンをViewに追加する.
        self.view.addSubview(myInfoDarkButton)
        self.view.addSubview(myInfoLightButton)
        self.view.addSubview(myAddButton)
        self.view.addSubview(myDetailButton)
        self.view.addSubview(mySystemButton)
        
    }

    override func didReceiveMemoryWarning() {
        super.didReceiveMemoryWarning()
        // Dispose of any resources that can be recreated.
    }
    
    /*
     ボタンイベント
     */
    internal func onClickMyButton(sender: UIButton) {
        print("onClickMyButton:")
        print("sender.currentTitile: \(sender.currentTitle)")
        print("sender.tag \(sender.tag)")
    }


}

```

## 2.3と3.0の差分

* UIColor.grayColor()がUIColor.grayに変更
* CGRectMake()がCGReat()に変更
* UIButton.setTitle()のforState: .Normalが for: .normalに変更
* UIButton.addTarget()の#selector(ViewController.onClickMyButton(_:))が、#selector(ViewController.onClickMyButton(sender:)に変更
* UIButton.addTarget()のforControlEvents: .TouchUpInsideがfor: .touchUpInsideに変更

## Reference

* UIColor
	* [https://developer.apple.com/reference/uikit/uicolor](https://developer.apple.com/reference/uikit/uicolor)
* UILabel
	* [https://developer.apple.com/reference/uikit/uibutton](https://developer.apple.com/reference/uikit/uibutton)
