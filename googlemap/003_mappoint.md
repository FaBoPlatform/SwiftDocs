# 指定した座標を表示

![Preview GMAP003](img/GMAP003.png)

## Swift3.0
### AppDelegate.swift
```swift
//
//  AppDelegate.swift
//  GoogleMaps003_3.0
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
//  GoogleMaps003_3.0
//
//  Created by KimikoWatanabe on 2016/08/30.
//  Copyright © 2016年 FaBo, Inc. All rights reserved.
//

import UIKit
import GoogleMaps

class ViewController: UIViewController, GMSMapViewDelegate {

    var googleMap : GMSMapView!

    override func viewDidLoad() {
        super.viewDidLoad()

        // 緯度.
        let lat: CLLocationDegrees = 37.508435

        // 経度.
        let lon: CLLocationDegrees = 139.930696

        // ズームレベル.
        let zoom: Float = 15

        // カメラを生成.
        let camera: GMSCameraPosition = GMSCameraPosition.camera(withLatitude: lat,longitude: lon, zoom: zoom)

        // MapViewを生成.
        googleMap = GMSMapView(frame: CGRect(x:0, y:0, width:self.view.bounds.width, height:self.view.bounds.height))

        // MapViewにカメラを追加.
        googleMap.camera = camera

        // viewにMapViewを追加.
        self.view.addSubview(googleMap)

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

    var googleMap : GMSMapView!

    override func viewDidLoad() {
        super.viewDidLoad()

        // 緯度.
        let lat: CLLocationDegrees = 37.508435

        // 経度.
        let lon: CLLocationDegrees = 139.930696

        // ズームレベル.
        let zoom: Float = 15

        // カメラを生成.
        let camera: GMSCameraPosition = GMSCameraPosition.cameraWithLatitude(lat,longitude: lon, zoom: zoom)

        // MapViewを生成.
        googleMap = GMSMapView(frame: CGRectMake(0, 0, self.view.bounds.width, self.view.bounds.height))

        // MapViewにカメラを追加.
        googleMap.camera = camera

        // viewにMapViewを追加.
        self.view.addSubview(googleMap)

    }

    override func didReceiveMemoryWarning() {
        super.didReceiveMemoryWarning()
        // Dispose of any resources that can be recreated.
    }
}
```

## 2.3と3.0の差分
* ```GMSCameraPosition.cameraWithLatitude```が```GMSCameraPosition.camera(withLatitude:```に変更
* CGRectの初期化方法の変更(CGRectMakeの廃止)

## Reference
* Google Map SDK for iOS
 * [https://developers.google.com/maps/documentation/ios/reference/index](https://developers.google.com/maps/documentation/ios/reference/index)
