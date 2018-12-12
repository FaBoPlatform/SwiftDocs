# UIButtonでいろいろなボタンを表示

![Preview uikit003](./img/uikit003.png)


## Swift 4.0

```swift
//
//  ViewController.swift
//  UIKit003
//
//  Copyright © 2018年 FaBo, Inc. All rights reserved.
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
        myInfoDarkButton = UIButton(type: .infoDark)
        myInfoLightButton = UIButton(type: .infoLight)
        myAddButton = UIButton(type: .contactAdd)
        myDetailButton = UIButton(type: .detailDisclosure)
        mySystemButton = UIButton(type: .system)

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
        mySystemButton.frame = CGRect(x: sposX, y: sposY, width: sWidth, height: sHeight)

        // Systemボタンにタイトルを設定する.
        mySystemButton.setTitle("MySystemButton", for: .normal)

        // タグを設定する.
        myInfoDarkButton.tag = 1
        myInfoLightButton.tag = 2
        myAddButton.tag = 3
        myDetailButton.tag = 4
        mySystemButton.tag = 5

        // イベントを追加する
        myInfoDarkButton.addTarget(self, action: #selector(ViewController.onClickMyButton(sender:)), for: .touchDown)
        myInfoLightButton.addTarget(self, action: #selector(ViewController.onClickMyButton(sender:)), for: .touchDown)
        myAddButton.addTarget(self, action: #selector(ViewController.onClickMyButton(sender:)), for: .touchDown)
        myDetailButton.addTarget(self, action: #selector(ViewController.onClickMyButton(sender:)), for: .touchDown)
        mySystemButton.addTarget(self, action: #selector(ViewController.onClickMyButton(sender:)), for: .touchDown)

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
    @objc internal func onClickMyButton(sender: UIButton) {
        print("onClickMyButton:")
        print("sender.currentTitile: \(sender.currentTitle)")
        print("sender.tag \(sender.tag)")
    }


}

```

## Swift 3.0

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
        myInfoDarkButton = UIButton(type: .infoDark)
        myInfoLightButton = UIButton(type: .infoLight)
        myAddButton = UIButton(type: .contactAdd)
        myDetailButton = UIButton(type: .detailDisclosure)
        mySystemButton = UIButton(type: .system)

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
        mySystemButton.frame = CGRect(x: sposX, y: sposY, width: sWidth, height: sHeight)

        // Systemボタンにタイトルを設定する.
        mySystemButton.setTitle("MySystemButton", for: .normal)

        // タグを設定する.
        myInfoDarkButton.tag = 1
        myInfoLightButton.tag = 2
        myAddButton.tag = 3
        myDetailButton.tag = 4
        mySystemButton.tag = 5

        // イベントを追加する
        myInfoDarkButton.addTarget(self, action: #selector(ViewController.onClickMyButton(sender:)), for: .touchDown)
        myInfoLightButton.addTarget(self, action: #selector(ViewController.onClickMyButton(sender:)), for: .touchDown)
        myAddButton.addTarget(self, action: #selector(ViewController.onClickMyButton(sender:)), for: .touchDown)
        myDetailButton.addTarget(self, action: #selector(ViewController.onClickMyButton(sender:)), for: .touchDown)
        mySystemButton.addTarget(self, action: #selector(ViewController.onClickMyButton(sender:)), for: .touchDown)

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

## 3.0と4.0の差分
* internal func onClickMyButton(sender: UIButton)が、
@objc internal func onClickMyButton(sender: UIButton)に変更

## 2.3と3.0の差分

* CGRectMake()がCGReat()に変更
* UIButton.addTarget()の#selector(ViewController.onClickMyButton(_:))が、#selector(ViewController.onClickMyButton(sender:)に変更
* UIButton.addTarget()のforControlEvents: .TouchDownがfor: .touchDownに変更

## Reference

* UIColor
	* [https://developer.apple.com/reference/uikit/uicolor](https://developer.apple.com/reference/uikit/uicolor)
* UIButton
	* [https://developer.apple.com/reference/uikit/uibutton](https://developer.apple.com/reference/uikit/uibutton)
