# UISegmentedControlの表示

![Preview uikit029](img/uikit029.png)

## Swift3.0
```swift
//
//  ViewController.swift
//  UIKit0_3.0
//
//  Created by KimikoWatanabe on 2016/08/13.
//  Copyright © 2016年 FaBo, Inc. All rights reserved.
//

```

## Swift 2.3
```swift
//
//  ViewController.swift
//  UIKit0_2.3
//
//  Created by KimikoWatanabe on 2016/08/16.
//  Copyright © 2016年 FaBo, Inc. All rights reserved.
//

import UIKit

class ViewController: UIViewController {

    private let mySegLabel: UILabel = UILabel(frame: CGRectMake(0,0,150,150))

    override func viewDidLoad() {
        super.viewDidLoad()

        // 表示する配列を作成する.
        let myArray: NSArray = ["Red","Blue","Green"]

        // SegmentedControlを作成する.
        let mySegcon: UISegmentedControl = UISegmentedControl(items: myArray as [AnyObject])
        mySegcon.center = CGPoint(x: self.view.frame.width/2, y: 400)
        mySegcon.backgroundColor = UIColor.grayColor()
        mySegcon.tintColor = UIColor.whiteColor()

        // イベントを追加する.
        mySegcon.addTarget(self, action: #selector(ViewController.segconChanged(_:)), forControlEvents: UIControlEvents.ValueChanged)

        // Viewに追加する.
        self.view.addSubview(mySegcon)

        // Labelを作成する.
        mySegLabel.backgroundColor = UIColor.whiteColor()
        mySegLabel.layer.masksToBounds = true
        mySegLabel.layer.cornerRadius = 75.0
        mySegLabel.textColor = UIColor.whiteColor()
        mySegLabel.shadowColor = UIColor.grayColor()
        mySegLabel.font = UIFont.systemFontOfSize(CGFloat(30))
        mySegLabel.textAlignment = NSTextAlignment.Center
        mySegLabel.layer.position = CGPoint(x: self.view.bounds.width/2,y: 200)

        // Viewの背景色をCyanにする.
        self.view.backgroundColor = UIColor.cyanColor()

        // Viewに追加する.
        self.view.addSubview(mySegLabel);
    }

    /*
     SwgmentedControlの値が変わったときに呼び出される.
     */
    internal func segconChanged(segcon: UISegmentedControl){

        switch segcon.selectedSegmentIndex {
        case 0:
            mySegLabel.backgroundColor = UIColor.redColor()

        case 1:
            mySegLabel.backgroundColor = UIColor.blueColor()

        case 2:
            mySegLabel.backgroundColor = UIColor.greenColor()

        default:
            print("Error")
        }
    }

    override func didReceiveMemoryWarning() {
        super.didReceiveMemoryWarning()
    }
}



```

## 2.3と3.0の差分


## Reference
