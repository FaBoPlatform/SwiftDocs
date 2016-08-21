# UICollectionViewをセクション毎に分ける

![Preview uikit055](img/uikit055.png)

## Swift3.0
### UIViewController.swift
```swift
//
//  ViewController.swift
//  UIKit055_3.0
//
//  Created by KimikoWatanabe on 2016/08/21.
//  Copyright © 2016年 FaBo, Inc. All rights reserved.
//

```
### CustomUICollectionViewCell.swift
```swift
//
//  CustomUICollectionViewCell.swift
//  UIKit055_3.0
//
//  Created by KimikoWatanabe on 2016/08/21.
//  Copyright © 2016年 FaBo, Inc. All rights reserved.
//

```

## Swift 2.3
### UIViewController.swift
```swift
//
//  ViewController.swift
//  UIKit055_2.3
//
//  Created by KimikoWatanabe on 2016/08/21.
//  Copyright © 2016年 FaBo, Inc. All rights reserved.
//

import UIKit

class ViewController: UIViewController, UICollectionViewDelegate, UICollectionViewDataSource {

    var myCollectionView : UICollectionView!

    override func viewDidLoad() {
        super.viewDidLoad()

        // CollectionViewのレイアウトを生成.
        let layout = UICollectionViewFlowLayout()

        // Cell一つ一つの大きさ.
        layout.itemSize = CGSizeMake(50, 50)

        // Cellのマージン.
        layout.sectionInset = UIEdgeInsetsMake(16, 16, 32, 16)

        // セクション毎のヘッダーサイズ.
        layout.headerReferenceSize = CGSizeMake(100,50)

        // CollectionViewを生成.
        myCollectionView = UICollectionView(frame: self.view.frame, collectionViewLayout: layout)

        // Cellに使われるクラスを登録.
        myCollectionView.registerClass(CustomUICollectionViewCell.self, forCellWithReuseIdentifier: "MyCell")
        myCollectionView.registerClass(UICollectionReusableView.self, forSupplementaryViewOfKind: UICollectionElementKindSectionHeader, withReuseIdentifier: "Section")

        myCollectionView.delegate = self
        myCollectionView.dataSource = self

        self.view.addSubview(myCollectionView)

    }

    /*
     Sectionの数
     */
    func numberOfSectionsInCollectionView(collectionView: UICollectionView) -> Int {
        return 3
    }

    /*
     Cellが選択された際に呼び出される
     */
    func collectionView(collectionView: UICollectionView, didSelectItemAtIndexPath indexPath: NSIndexPath) {

        print("Num: \(indexPath.row)")
        print("SectionNum:\(indexPath.section)")

    }

    /*
     Cellの総数を返す
     */
    func collectionView(collectionView: UICollectionView, numberOfItemsInSection section: Int) -> Int {

        // Section毎にCellの総数を変える.
        switch(section){
        case 0:
            return 5

        case 1:
            return 8

        case 2:
            return 20

        default:
            print("error")
            return 0
        }

    }

    /*
     Sectionに値を設定する
     */
    func collectionView(collectionView: UICollectionView, viewForSupplementaryElementOfKind kind: String, atIndexPath indexPath: NSIndexPath) -> UICollectionReusableView {

        let headerView = collectionView.dequeueReusableSupplementaryViewOfKind(UICollectionElementKindSectionHeader, withReuseIdentifier: "Section", forIndexPath: indexPath)

        headerView.backgroundColor = UIColor.whiteColor()

        return headerView
    }

    /*
     Cellに値を設定する
     */
    func collectionView(collectionView: UICollectionView, cellForItemAtIndexPath indexPath: NSIndexPath) -> UICollectionViewCell {

        let cell : CustomUICollectionViewCell = collectionView.dequeueReusableCellWithReuseIdentifier("MyCell", forIndexPath: indexPath) as! CustomUICollectionViewCell

        // Section毎にCellのプロパティを変える.
        switch(indexPath.section){
        case 0:
            cell.backgroundColor = UIColor.redColor()
            cell.textLabel?.text = "0"

        case 1:
            cell.backgroundColor = UIColor.greenColor()
            cell.textLabel?.text = "1"

        case 2:
            cell.backgroundColor = UIColor.blueColor()
            cell.textLabel?.text = "2"

        default:
            print("section error")
            cell.backgroundColor = UIColor.whiteColor()
        }

        return cell
    }

}
```

### CustomUICollectionViewCell.swift
```swift
//
//  CustomUICollectionViewCell.swift
//  UIKit055_2.3
//
//  Created by KimikoWatanabe on 2016/08/21.
//  Copyright © 2016年 FaBo, Inc. All rights reserved.
//

import UIKit

class CustomUICollectionViewCell : UICollectionViewCell{

    var textLabel : UILabel?

    required init(coder aDecoder: NSCoder) {
        super.init(coder: aDecoder)!
    }

    override init(frame: CGRect) {
        super.init(frame: frame)

        // UILabelを生成.
        textLabel = UILabel(frame: CGRectMake(0, 0, frame.width, frame.height))
        textLabel?.text = "nil"
        textLabel?.textAlignment = NSTextAlignment.Center

        // Cellに追加.
        self.contentView.addSubview(textLabel!)
    }

}
```


## 2.3と3.0の差分
* UIColorの参照方法が変更(UIColor.grayColor()->UIColor.gray)
* CGRect,CGPointの初期化方法の変更(CGRectMake,CGPointMakeの廃止)

## Reference
* UICollectionViewFlowLayout Class
* UICollectionView Class
* UIEdgeInsetsMake
* UICollectionDelegate
* UICollectionDataSource
* UIKit Class
