# バッテリーの残量を取得する

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

    override func viewDidLoad() {
        super.viewDidLoad()

        //デバイスとバッテリー残量の宣言.
        let myDevice: UIDevice = UIDevice.currentDevice()

        //バッテリー状態の監視.
        myDevice.batteryMonitoringEnabled = true

        //バッテリープロパティ表示用のラベル.
        let myBatteryLevel = myDevice.batteryLevel
        let myBatteryState = myDevice.batteryState

        //バッテリー状態の取得.
        //0.0~1.0で残量表示。残量不明の時は-1.0を返す.
        print("\(myBatteryLevel) %")

        switch (myBatteryState) {

        case .Full:
            self.view.backgroundColor = UIColor.cyanColor()

        case .Unplugged:
            self.view.backgroundColor = UIColor.redColor()

        case .Charging:
            self.view.backgroundColor = UIColor.blueColor()

        case .Unknown:
            self.view.backgroundColor = UIColor.grayColor()

        }

        // Labelを作成.
        let myLabel = UILabel(frame: CGRectMake(0,0,200,200))
        myLabel.backgroundColor = UIColor.orangeColor()
        myLabel.layer.masksToBounds = true
        myLabel.layer.cornerRadius = 100.0
        myLabel.text = "\(myBatteryLevel * 100) %"
        myLabel.textColor = UIColor.whiteColor()
        myLabel.shadowColor = UIColor.grayColor()
        myLabel.textAlignment = NSTextAlignment.Center
        myLabel.layer.position = self.view.center
        self.view.addSubview(myLabel)

    }

}
```

## 2.3と3.0の差分
* UIColorの参照方法が変更(UIColor.grayColor()->UIColor.gray)
* CGRect,CGPointの初期化方法の変更(CGRectMake,CGPointMakeの廃止)

## Reference
