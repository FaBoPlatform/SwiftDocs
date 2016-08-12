# UITableViewでテーブルを表示

![Preview uikit006](./img/uikit006.png)


## Swift 3.0

```swift
//
//  ViewController.swift
//  UIKit005
//
//  Copyright © 2016年 FaBo, Inc. All rights reserved.
//

import UIKit

class ViewController: UIViewController {
    
    private var myImageView: UIImageView!
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        // UIImageViewのサイズを設定する
        let iWidth: CGFloat = 300
        let iHeight: CGFloat = 100
        
        // UIImageViewのx,yを設定する
        let posX: CGFloat = (self.view.bounds.width - iWidth)/2
        let posY: CGFloat = (self.view.bounds.height - iHeight)/2
        
        // UIImageViewを作成.
        myImageView = UIImageView(frame: CGRect(x: posX, y: posY, width: iWidth, height: iHeight))
        
        // UIImageを作成.
        let myImage: UIImage = UIImage(named: "fabo_logo.png")!
        
        // 画像をUIImageViewに設定する.
        myImageView.image = myImage
        
        
        // UIImageViewをViewに追加する
        self.view.addSubview(myImageView)
    }
    
    override func didReceiveMemoryWarning() {
        super.didReceiveMemoryWarning()
        // Dispose of any resources that can be recreated.
    }
    
}



```

## Swift 2.3

```swift
//
//  ViewController.swift
//  UIKit006
//
//  Copyright © 2016年 FaBo, Inc. All rights reserved.
//

import UIKit

class ViewController: UIViewController, UITableViewDelegate, UITableViewDataSource {

    // Tableで使用する配列を設定する
    private let myItems: NSArray = ["TEST1", "TEST2", "TEST3"]
    private var myTableView: UITableView!
    
    override func viewDidLoad() {
        super.viewDidLoad()

        // Status Barの高さを取得する.
        let barHeight: CGFloat = UIApplication.sharedApplication().statusBarFrame.size.height
        
        // Viewの高さと幅を取得する.
        let displayWidth: CGFloat = self.view.frame.width
        let displayHeight: CGFloat = self.view.frame.height
        
        // TableViewの生成(Status barの高さをずらして表示).
        myTableView = UITableView(frame: CGRectMake(0, barHeight, displayWidth, displayHeight))
        
        // Cell名の登録をおこなう.
        myTableView.registerClass(UITableViewCell.self, forCellReuseIdentifier: "MyCell")
        
        // DataSourceを自身に設定する.
        myTableView.dataSource = self
        
        // Delegateを自身に設定する.
        myTableView.delegate = self
        
        // Viewに追加する.
        self.view.addSubview(myTableView)
        
    }

    override func didReceiveMemoryWarning() {
        super.didReceiveMemoryWarning()
        // Dispose of any resources that can be recreated.
    }
    
    /*
     Cellが選択された際に呼び出される
    */
    func tableView(tableView: UITableView, didSelectRowAtIndexPath indexPath: NSIndexPath) {
        print("Num: \(indexPath.row)")
        print("Value: \(myItems[indexPath.row])")
    }

    /*
     Cellの総数を返す.
     */
    func tableView(tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
        return myItems.count
    }
    
    /*
     Cellに値を設定する
    */
    func tableView(tableView: UITableView, cellForRowAtIndexPath indexPath: NSIndexPath) -> UITableViewCell {
        // 再利用するCellを取得する.
        let cell = tableView.dequeueReusableCellWithIdentifier("MyCell", forIndexPath: indexPath)
        
        // Cellに値を設定する.
        cell.textLabel!.text = "\(myItems[indexPath.row])"
        
        return cell
    }
    
}

```

## 2.3と3.0の差分

* CGRectMake()がCGRect()に変更.


## Reference

* UIImageView
	* [https://developer.apple.com/reference/uikit/uiimageview](https://developer.apple.com/reference/uikit/uiimageview)
* UIImage
    * [https://developer.apple.com/reference/uikit/uiimage](https://developer.apple.com/reference/uikit/uiimage)
