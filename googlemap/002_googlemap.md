# GoogleMapを表示する

![Preview GMAP002](img/GMAP002.png)

## Swift3.0
### AppDelegate.swift
```swift
//
//  AppDelegate.swift
//  GoogleMaps002_3.0
//
//  Created by KimikoWatanabe on 2016/08/30.
//  Copyright © 2016年 FaBo, Inc. All rights reserved.
//

import UIKit
import GoogleMaps

@UIApplicationMain
class AppDelegate: UIResponder, UIApplicationDelegate {

    var window: UIWindow?


    func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [NSObject: AnyObject]?) -> Bool {

        GMSServices.provideAPIKey("取得したAPIKey")

        // Override point for customization after application launch.
        return true
    }
}
```
### ViewController.swift
```swift
//
//  ViewController.swift
//  GoogleMaps002_3.0
//
//  Created by KimikoWatanabe on 2016/08/30.
//  Copyright © 2016年 FaBo, Inc. All rights reserved.
//

import UIKit
import GoogleMaps

class ViewController: UIViewController, GMSMapViewDelegate {

    var gmaps : GMSMapView!

    override func viewDidLoad() {
        super.viewDidLoad()

        // MapViewを生成する.
        gmaps = GMSMapView(frame: CGRect(x:0, y:0, width:self.view.bounds.width, height:self.view.bounds.height))

        // MapViewをviewに追加する.
        self.view.addSubview(gmaps)

    }

    override func didReceiveMemoryWarning() {
        super.didReceiveMemoryWarning()
        // Dispose of any resources that can be recreated.
    }
}
```

## Swift 2.3
### AppDelegate.swift
```swift
//
//  AppDelegate.swift
//  GoogleMaps002_2.3
//
//  Created by KimikoWatanabe on 2016/08/30.
//  Copyright © 2016年 FaBo, Inc. All rights reserved.
//

import UIKit

@UIApplicationMain
class AppDelegate: UIResponder, UIApplicationDelegate {

    var window: UIWindow?


    func application(application: UIApplication, didFinishLaunchingWithOptions launchOptions: [NSObject: AnyObject]?) -> Bool {

        GMSServices.provideAPIKey("取得したAPIKey")

        // Override point for customization after application launch.
        return true
    }
}
```
### ViewController.swift
```swift
//
//  ViewController.swift
//  GoogleMaps002_2.3
//
//  Created by KimikoWatanabe on 2016/08/22.
//  Copyright © 2016年 FaBo, Inc. All rights reserved.
//

import UIKit

class ViewController: UIViewController, GMSMapViewDelegate {

    var gmaps : GMSMapView!

    override func viewDidLoad() {
        super.viewDidLoad()

        // MapViewを生成する.
        gmaps = GMSMapView(frame: CGRectMake(0, 0, self.view.bounds.width, self.view.bounds.height))

        // MapViewをviewに追加する.
        self.view.addSubview(gmaps)

    }

    override func didReceiveMemoryWarning() {
        super.didReceiveMemoryWarning()
        // Dispose of any resources that can be recreated.
    }
}
```

## 2.3と3.0の差分
* CGRectの初期化方法の変更(CGRectMakeの廃止)

## Reference
