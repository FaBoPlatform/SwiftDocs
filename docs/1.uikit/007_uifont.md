# UIFontで文字を表示

![Preview uikit007](./img/uikit007.png)


```swift fct_label="Swift 4.x/Swift 3.x"
//
//  ViewController.swift
//  UIKit007
//
//  Copyright © 2016年 FaBo, Inc. All rights reserved.
//

import UIKit

class ViewController: UIViewController {
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        // 小さめのフォントの文字列をラベルに表示する.
        let mySmallLabel: UILabel = UILabel(frame: CGRect(x: 25, y: 0, width: 300, height: 150))
        mySmallLabel.text = "小さめのフォント"
        mySmallLabel.font = UIFont.systemFont(ofSize: UIFont.smallSystemFontSize)
        self.view.addSubview(mySmallLabel)
        
        // システムの標準のフォントサイズの文字列をラベルを表示する.
        let myNormalLabel: UILabel = UILabel(frame: CGRect(x: 25, y: 30, width: 200, height: 150))
        myNormalLabel.font = UIFont.systemFont(ofSize: UIFont.systemFontSize)
        myNormalLabel.text = "システム標準のフォントサイズ"
        self.view.addSubview(myNormalLabel)
        
        // UIButton用のフォントサイズの文字列をラベルに表示する.
        let myButtonLabel: UILabel = UILabel(frame: CGRect(x: 25, y: 60, width: 300, height: 150))
        myButtonLabel.font = UIFont.systemFont(ofSize: UIFont.buttonFontSize)
        myButtonLabel.text = "UIButtonのフォントサイズ"
        self.view.addSubview(myButtonLabel)
        
        // カスタムしたフォントサイズ(20)の文字列をラベルに表示する.
        let myCustomLabel: UILabel = UILabel(frame: CGRect(x: 25, y: 90, width: 300, height: 150))
        myCustomLabel.font = UIFont.systemFont(ofSize: CGFloat(20))
        myCustomLabel.text = "ポイント20のフォントサイズ"
        self.view.addSubview(myCustomLabel)
        
        // Italic Sysrem Fontの文字列をラベルに表示する.
        let myItalicLabel: UILabel = UILabel(frame: CGRect(x: 25, y: 150, width: 300, height: 150))
        myItalicLabel.font = UIFont.italicSystemFont(ofSize: UIFont.labelFontSize)
        myItalicLabel.text = "Italicフォント"
        self.view.addSubview(myItalicLabel)
        
        // Boldの文字列をラベルに表示する.
        let myBoldLabel: UILabel = UILabel(frame: CGRect(x: 25, y: 180, width: 300, height: 150))
        myBoldLabel.font = UIFont.boldSystemFont(ofSize: UIFont.labelFontSize)
        myBoldLabel.text = "Boldフォント"
        self.view.addSubview(myBoldLabel)
        
        // Arialの文字列をラベルに表示する.
        let myArialLabel: UILabel = UILabel(frame: CGRect(x: 25, y: 230, width: 300, height: 150))
        myArialLabel.font = UIFont(name: "ArilHebew", size: UIFont.labelFontSize)
        myArialLabel.text = "ArialHebrew"
        self.view.addSubview(myArialLabel)
        
    }
    
    override func didReceiveMemoryWarning() {
        super.didReceiveMemoryWarning()
        // Dispose of any resources that can be recreated.
    }
    
    
}


```


```swift fct_label="Swift 2.3"
//
//  ViewController.swift
//  UIKit007
//
//  Copyright © 2016年 FaBo, Inc. All rights reserved.
//

import UIKit

class ViewController: UIViewController {

    override func viewDidLoad() {
        super.viewDidLoad()
        
        // 小さめのフォントの文字列をラベルに表示する.
        let mySmallLabel: UILabel = UILabel(frame: CGRectMake(25, 0, 300, 150))
        mySmallLabel.text = "小さめのフォント"
        mySmallLabel.font = UIFont.systemFontOfSize(UIFont.smallSystemFontSize())
        self.view.addSubview(mySmallLabel)
        
        // システムの標準のフォントサイズの文字列をラベルを表示する.
        let myNormalLabel: UILabel = UILabel(frame: CGRectMake(25, 30, 200, 150))
        myNormalLabel.font = UIFont.systemFontOfSize(UIFont.systemFontSize())
        myNormalLabel.text = "システム標準のフォントサイズ"
        self.view.addSubview(myNormalLabel)
        
        // UIButton用のフォントサイズの文字列をラベルに表示する.
        let myButtonLabel: UILabel = UILabel(frame: CGRectMake(25, 60, 300, 150))
        myButtonLabel.font = UIFont.systemFontOfSize(UIFont.buttonFontSize())
        myButtonLabel.text = "UIButtonのフォントサイズ"
        self.view.addSubview(myButtonLabel)
        
        // カスタムしたフォントサイズ(20)の文字列をラベルに表示する.
        let myCustomLabel: UILabel = UILabel(frame: CGRectMake(25, 90, 300, 150))
        myCustomLabel.font = UIFont.systemFontOfSize(CGFloat(20))
        myCustomLabel.text = "ポイント20のフォントサイズ"
        self.view.addSubview(myCustomLabel)
        
        // Italic Sysrem Fontの文字列をラベルに表示する.
        let myItalicLabel: UILabel = UILabel(frame: CGRectMake(25, 150, 300, 150))
        myItalicLabel.font = UIFont.italicSystemFontOfSize(UIFont.labelFontSize())
        myItalicLabel.text = "Italicフォント"
        self.view.addSubview(myItalicLabel)
        
        // Boldの文字列をラベルに表示する.
        let myBoldLabel: UILabel = UILabel(frame: CGRectMake(25, 180, 300, 150))
        myBoldLabel.font = UIFont.boldSystemFontOfSize(UIFont.labelFontSize())
        myBoldLabel.text = "Boldフォント"
        self.view.addSubview(myBoldLabel)
        
        // Arialの文字列をラベルに表示する.
        let myArialLabel: UILabel = UILabel(frame: CGRectMake(25, 230, 300, 150))
        myArialLabel.font = UIFont(name: "ArilHebew", size: UIFont.labelFontSize())
        myArialLabel.text = "ArialHebrew"
        self.view.addSubview(myArialLabel)
        
    }

    override func didReceiveMemoryWarning() {
        super.didReceiveMemoryWarning()
        // Dispose of any resources that can be recreated.
    }


}
```

## 2.3と3.0の差分

* CGRectMake()がCGRect()に変更.
* UIFont.systemFontOfSize(UIFont.smallSystemFontSize())がUIFont.systemFont(ofSize: UIFont.smallSystemFontSize)に変更.

## Reference

* UIFont
	* [https://developer.apple.com/reference/uikit/uifont](https://developer.apple.com/reference/uikit/uifont)