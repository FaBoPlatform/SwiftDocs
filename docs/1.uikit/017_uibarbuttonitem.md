# UIBarButtonItemの設置

![Preview uikit017_001](./img/uikit017_001.png)
![Preview uikit017_002](./img/uikit017_002.png)
![Preview uikit017_003](./img/uikit017_003.png)

```swift fct_label="Swift 4.x/Swift 3.x"
//
//  AppDelegate.swift
//  UIKit017_3.0
//
//  Created by KimikoWatanabe on 2016/08/16.
//  Copyright © 2016年 FaBo, Inc. All rights reserved.
//

import UIKit

@UIApplicationMain
class AppDelegate: UIResponder, UIApplicationDelegate {

    internal var window: UIWindow?

    func applicationDidFinishLaunching(_ application: UIApplication) {

        // viewConrollerを生成.
        let myViewController = ViewController()

        // navigationControllerを生成.
        let navigationController = UINavigationController(rootViewController: myViewController)

        // windowを生成.
        window = UIWindow(frame: UIScreen.main.bounds)

        // rootViewControllerにnavigationControllerを設定.
        window?.rootViewController = navigationController

        window?.makeKeyAndVisible()

        //return true
    }

}
```

```swift fct_label="Swift 2.3"
//
//  AppDelegate.swift
//  UIKit017_2.3
//
//  Created by KimikoWatanabe on 2016/08/16.
//  Copyright © 2016年 FaBo, Inc. All rights reserved.
//

import UIKit

@UIApplicationMain
class AppDelegate: UIResponder, UIApplicationDelegate {

    internal var window: UIWindow?

    func application(application: UIApplication, didFinishLaunchingWithOptions launchOptions: [NSObject: AnyObject]?) -> Bool {

        // viewConrollerを生成.
        let myViewController = ViewController()

        // navigationControllerを生成.
        let navigationController = UINavigationController(rootViewController: myViewController)

        // windowを生成.
        window = UIWindow(frame: UIScreen.mainScreen().bounds)

        // rootViewControllerにnavigationControllerを設定.
        window?.rootViewController = navigationController

        window?.makeKeyAndVisible()
        return true
    }

}
```

### ViewController.swift
```swift fct_label="Swift 4.x"
//
//  ViewController.swift
//  UIKit017_4.0
//
//  Created by KimikoWatanabe on 2016/08/16.
//  Copyright © 2018年 FaBo, Inc. All rights reserved.
//

import UIKit

class ViewController: UIViewController {

    private var myLeftButton: UIBarButtonItem!
    private var myRightButton: UIBarButtonItem!

    override func viewDidLoad() {
        super.viewDidLoad()

        // 背景色をCyanに設定する.
        self.view.backgroundColor = UIColor.cyan

        // NavigationControllerのタイトルを設定する.
        self.title = "My Navigation"

        // 左ボタンを作成する..
        myLeftButton = UIBarButtonItem(barButtonSystemItem: .add, target: self, action: #selector(ViewController.onClickMyButton(sender:)))

        // 右ボタンを作成する.
        myRightButton = UIBarButtonItem(title: "RightBtn", style: .plain, target: self, action: #selector(ViewController.onClickMyButton(sender:)))

        // tagを設定する.
        myLeftButton.tag = 1
        myRightButton.tag = 2

        // ナビゲーションバーの左に設置する.
        self.navigationItem.leftBarButtonItem = myLeftButton

        // ナビゲーションバーの右に設置する.
        self.navigationItem.rightBarButtonItem = myRightButton
    }

    /*
     ボタンイベント.
     */
    @objc internal func onClickMyButton(sender: UIButton){

        switch(sender.tag){

        case 1:
            // 背景色を青色に変える.
            self.view.backgroundColor = UIColor.blue

        case 2:
            // 背景色を赤色に変える.
            self.view.backgroundColor = UIColor.red

        default:
            print("error")
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
//  UIKit017_3.0
//
//  Created by KimikoWatanabe on 2016/08/16.
//  Copyright © 2016年 FaBo, Inc. All rights reserved.
//

import UIKit

class ViewController: UIViewController {

    private var myLeftButton: UIBarButtonItem!
    private var myRightButton: UIBarButtonItem!

    override func viewDidLoad() {
        super.viewDidLoad()

        // 背景色をCyanに設定する.
        self.view.backgroundColor = UIColor.cyan

        // NavigationControllerのタイトルを設定する.
        self.title = "My Navigation"

        // 左ボタンを作成する..
        myLeftButton = UIBarButtonItem(barButtonSystemItem: .add, target: self, action: #selector(ViewController.onClickMyButton(sender:)))

        // 右ボタンを作成する.
        myRightButton = UIBarButtonItem(title: "RightBtn", style: .plain, target: self, action: #selector(ViewController.onClickMyButton(sender:)))

        // tagを設定する.
        myLeftButton.tag = 1
        myRightButton.tag = 2

        // ナビゲーションバーの左に設置する.
        self.navigationItem.leftBarButtonItem = myLeftButton

        // ナビゲーションバーの右に設置する.
        self.navigationItem.rightBarButtonItem = myRightButton
    }

    /*
     ボタンイベント.
     */
    internal func onClickMyButton(sender: UIButton){

        switch(sender.tag){

        case 1:
            // 背景色を青色に変える.
            self.view.backgroundColor = UIColor.blue

        case 2:
            // 背景色を赤色に変える.
            self.view.backgroundColor = UIColor.red

        default:
            print("error")
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
//  UIKit017_2.3
//
//  Created by KimikoWatanabe on 2016/08/16.
//  Copyright © 2016年 FaBo, Inc. All rights reserved.
//

import UIKit

class ViewController: UIViewController {

    private var myLeftButton: UIBarButtonItem!
    private var myRightButton: UIBarButtonItem!

    override func viewDidLoad() {
        super.viewDidLoad()

        // 背景色をCyanに設定する.
        self.view.backgroundColor = UIColor.cyanColor()

        // NavigationControllerのタイトルを設定する.
        self.title = "My Navigation"

        // 左ボタンを作成する..
        myLeftButton = UIBarButtonItem(barButtonSystemItem: .Add, target: self, action: "onClickMyButton:")

        // 右ボタンを作成する.
        myRightButton = UIBarButtonItem(title: "RightBtn", style: .Plain, target: self, action: "onClickMyButton:")

        // tagを設定する.
        myLeftButton.tag = 1
        myRightButton.tag = 2

        // ナビゲーションバーの左に設置する.
        self.navigationItem.leftBarButtonItem = myLeftButton

        // ナビゲーションバーの右に設置する.
        self.navigationItem.rightBarButtonItem = myRightButton
    }

    /*
     ボタンイベント.
     */
    internal func onClickMyButton(sender: UIButton){

        switch(sender.tag){

        case 1:
            // 背景色を青色に変える.
            self.view.backgroundColor = UIColor.blueColor()

        case 2:
            // 背景色を赤色に変える.
            self.view.backgroundColor = UIColor.redColor()

        default:
            print("error")
        }
    }

    override func didReceiveMemoryWarning() {
        super.didReceiveMemoryWarning()
    }
}
```

## 3.0と4.0の差分
* internal func onClickMyButton(sender: UIButton)が
@objc internal func onClickMyButton(sender: UIButton)に変更

## 2.3と3.0の差分
* UIApplicationDelegateのDelegateメソッド名が変更
* UIScreenからスクリーンのサイズ取得の方法が変更(UIScreen.mainScreen() -> UIScreen.main)
* UIBarButtonItemStyleの名称が変更

## Reference
* UIBarButtonItem Class
 * [https://developer.apple.com/reference/uikit/uibarbuttonitem](https://developer.apple.com/reference/uikit/uibarbuttonitem)
* UINavigationController Class
 * [https://developer.apple.com/reference/uikit/uinavigationcontroller](https://developer.apple.com/reference/uikit/uinavigationcontroller)
* UIViewController Class
 * [https://developer.apple.com/reference/uikit/uiviewcontroller](https://developer.apple.com/reference/uikit/uiviewcontroller)
