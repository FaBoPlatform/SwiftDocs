# UILabelで文字を表示

![Preview uikit001](./img/uikit001.png)

```swift  fct_label="Swift 5.x/4.x/3.x"
//
//  ViewController.swift
//  uikit001
//
//  Copyright © 2016年 FaBo, Inc. All rights reserved.
//

import UIKit

class ViewController: UIViewController {

    override func viewDidLoad() {
        super.viewDidLoad()

        // ボタンのサイズを定義.
        let bWidth: CGFloat = 200
        let bHeight: CGFloat = 50

        // 配置する座標を定義(画面の中心).
        let posX: CGFloat = self.view.bounds.width/2 - bWidth/2
        let posY: CGFloat = self.view.bounds.height/2 - bHeight/2

        // Labelを作成.
        let label: UILabel = UILabel(frame: CGRect(x: posX, y: posY, width: bWidth, height: bHeight))

        // UILabelの背景をオレンジ色に.
        label.backgroundColor = UIColor.orange

        // UILabelの枠を丸くする.
        label.layer.masksToBounds = true

        // 丸くするコーナーの半径.
        label.layer.cornerRadius = 20.0

        // 文字の色を白に定義.
        label.textColor = UIColor.white

        // UILabelに文字を代入.
        label.text = "Hello Swift!!"

        // 文字の影をグレーに定義.
        label.shadowColor = UIColor.gray

        // Textを中央寄せにする.
        label.textAlignment = NSTextAlignment.center

        // Viewの背景を青にする.
        self.view.backgroundColor = UIColor.cyan

        // ViewにLabelを追加.
        self.view.addSubview(label)

    }

    override func didReceiveMemoryWarning() {
        super.didReceiveMemoryWarning()
        // Dispose of any resources that can be recreated.
    }


}
```

```swift fct_label="Swift UI"
//
//  ContentView.swift
//
//  Copyright © 2021年 FaBo, Inc. All rights reserved.
//

import SwiftUI

struct ContentView: View {
    var body: some View {
        ZStack {
            Color.init(UIColor.cyan).edgesIgnoringSafeArea(.all)
            
            let bWidth: CGFloat = 200
            let bHeight: CGFloat = 50

            Text("Hello Swift!!")
                .frame(maxWidth: bWidth, maxHeight: bHeight, alignment: .center)
                .foregroundColor(Color.white)
                .background(Color.orange)
                .shadow(color: .gray, radius: 10, x: 0.0, y: 0.0)
                .cornerRadius(10)
        }
    }
}
```

## Reference

* UIColor
	* [https://developer.apple.com/reference/uikit/uicolor](https://developer.apple.com/reference/uikit/uicolor)
* UILabel
	* [https://developer.apple.com/reference/uikit/uilabel](https://developer.apple.com/reference/uikit/uilabel)
