# UIScrollViewの表示

![Preview uikit016](./img/uikit016.png)

## Swift4.0
```swift

//
//  ViewController.swift
//  UIKit016_3.0
//
//  Created by KimikoWatanabe on 2016/08/16.
//  Copyright © 2018年 FaBo, Inc. All rights reserved.
//

import UIKit

class ViewController: UIViewController {
    
    private var myScrollView: UIScrollView!
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        // ScrollViewを生成.
        myScrollView = UIScrollView()
        
        // ScrollViewの大きさを設定する.
        myScrollView.frame = self.view.frame
        
        // UIImageに画像を設定する.
        let myImage = UIImage(named: "temple.jpg")!
        
        // UIImageViewを生成する.
        let myImageView = UIImageView()
        
        // myImageViewのimageにmyImageを設定する.
        myImageView.image = myImage
        
        // frameの値を設定する.
        myImageView.frame = myScrollView.frame
        
        // 画像のアスペクト比を設定.
        myImageView.contentMode = UIView.ContentMode.scaleAspectFill

        // ScrollViewにmyImageViewを追加する.
        myScrollView.addSubview(myImageView)
        
        // Scrollの高さを計算しておく.
        let scroll_height = myImage.size.height*(self.view.frame.width/myImage.size.width)
        
        // ScrollViewにcontentSizeを設定する.
        myScrollView.contentSize = CGSize(width:self.view.frame.width, height:scroll_height)
        
        // ViewにScrollViewをAddする.
        self.view.addSubview(myScrollView)
    }
    
    override func didReceiveMemoryWarning() {
        super.didReceiveMemoryWarning()
    }
    
}


```

## Swift3.0
```swift
//
//  ViewController.swift
//  UIKit016_3.0
//
//  Created by KimikoWatanabe on 2016/08/16.
//  Copyright © 2016年 FaBo, Inc. All rights reserved.
//

import UIKit

class ViewController: UIViewController {

    private var myScrollView: UIScrollView!

    override func viewDidLoad() {
        super.viewDidLoad()

        // ScrollViewを生成.
        myScrollView = UIScrollView()

        // ScrollViewの大きさを設定する.
        myScrollView.frame = self.view.frame

        // UIImageに画像を設定する.
        let myImage = UIImage(named: "temple.jpg")!

        // UIImageViewを生成する.
        let myImageView = UIImageView()

        // myImageViewのimageにmyImageを設定する.
        myImageView.image = myImage

        // frameの値を設定する.
        myImageView.frame = myScrollView.frame

        // 画像のアスペクト比を設定.
        myImageView.contentMode = UIViewContentMode.scaleAspectFill

        // ScrollViewにmyImageViewを追加する.
        myScrollView.addSubview(myImageView)

        // Scrollの高さを計算しておく.
        let scroll_height = myImage.size.height*(self.view.frame.width/myImage.size.width)

        // ScrollViewにcontentSizeを設定する.
        myScrollView.contentSize = CGSize(width:self.view.frame.width, height:scroll_height)

        // ViewにScrollViewをAddする.
        self.view.addSubview(myScrollView)
    }

    override func didReceiveMemoryWarning() {
        super.didReceiveMemoryWarning()
    }

}

```


## Swift 2.3
```swift
//
//  ViewController.swift
//  UIKit016_2.3
//
//  Created by KimikoWatanabe on 2016/08/16.
//  Copyright © 2016年 FaBo, Inc. All rights reserved.

import UIKit

class ViewController: UIViewController {

    private var myScrollView: UIScrollView!

    override func viewDidLoad() {
        super.viewDidLoad()

        // ScrollViewを生成.
        myScrollView = UIScrollView()

        // ScrollViewの大きさを設定する.
        myScrollView.frame = self.view.frame

        // UIImageに画像を設定する.
        let myImage = UIImage(named: "temple.jpg")!

        // UIImageViewを生成する.
        let myImageView = UIImageView()

        // myImageViewのimageにmyImageを設定する.
        myImageView.image = myImage

        // frameの値を設定する.
        myImageView.frame = myScrollView.frame

        // 画像のアスペクト比を設定.
        myImageView.contentMode = UIViewContentMode.ScaleAspectFill

        // ScrollViewにmyImageViewを追加する.
        myScrollView.addSubview(myImageView)

        // Scrollの高さを計算しておく.
        let scroll_height = myImage.size.height*(self.view.frame.width/myImage.size.width)

        // ScrollViewにcontentSizeを設定する.
        myScrollView.contentSize = CGSizeMake(self.view.frame.width, scroll_height)

        // ViewにScrollViewをAddする.
        self.view.addSubview(myScrollView)
    }

    override func didReceiveMemoryWarning() {
        super.didReceiveMemoryWarning()
    }

}

```
![Preview temple.jpg](./img/temple.jpg)

## 3.0と4.0の差分
* UIViewContentMode.scaleAspectFillが、
UIView.ContentMode.scaleAspectFillに変更

## 2.3と3.0の差分
* UIViewContentModeの名称が変更
* CGSizeの生成方法の変更(CGSizeMakeの廃止)

## Reference
* UIScrollView Class
 * [https://developer.apple.com/reference/uikit/uiscrollview](https://developer.apple.com/reference/uikit/uiscrollview)
* UIImageView Class
 * [https://developer.apple.com/reference/uikit/uiimageview](https://developer.apple.com/reference/uikit/uiimageview)
* UIImage Class
 * [https://developer.apple.com/reference/uikit/uiimage](https://developer.apple.com/reference/uikit/uiimage)
