# 三角形を描画する

![Preview spritekit023](img/spritekit023.png)

## Swift3.0
### GameScene.swift
```swift
//
//  GameScene.swift
//  SpriteKit023
//
//  Created by Misato Morino on 2016/09/20.
//  Copyright © 2016年 Misato Morino. All rights reserved.
//

import SpriteKit

class GameScene: SKScene{
    
    override func didMove(to view: SKView) {
        
        // 1辺の大きさ.
        let length: CGFloat = 50
        
        // 始点から終点までの４点を指定.
        var points = [CGPoint(x:length, y:-length / 2.0),
                      CGPoint(x:-length, y:-length / 2.0),
                      CGPoint(x: 0.0, y: length),
                      CGPoint(x:length, y:-length / 2.0)]
        
        // 座標から三角形のSKShapeNodeを生成.
        let Triangle = SKShapeNode(points: &points, count: points.count)
        
        // 塗りつぶしの色を赤色に指定.
        Triangle.fillColor = UIColor.red
        
        // 座標をsceneの中心に指定.
        Triangle.position = CGPoint(x: self.frame.midX, y: self.frame.midY)
        
        // ShapeNodeをsceneに追加.
        self.addChild(Triangle)
    }
} 
```

## Swift 2.3
### GameScene.swift
```swift 
//
//  GameScene.swift
//  SpriteKit023
//
//  Created by Misato Morino on 2016/09/20.
//  Copyright © 2016年 Misato Morino. All rights reserved.
//

import SpriteKit

class GameScene: SKScene{
    
    override func didMoveToView(view: SKView) {
        
        // 1辺の大きさ.
        let length: CGFloat = 50
        
        // 始点から終点までの４点を指定.
        var points = [CGPoint(x:length, y:-length / 2.0),
                      CGPoint(x:-length, y:-length / 2.0),
                      CGPoint(x: 0.0, y: length),
                      CGPoint(x:length, y:-length / 2.0)]
        
        // 座標から三角形のSKShapeNodeを生成.
        let Triangle = SKShapeNode(points: &points, count: points.count)
        
        // 塗りつぶしの色を赤色に指定.
        Triangle.fillColor = UIColor.redColor()
        
        // 座標をsceneの中心に指定.
        Triangle.position = CGPointMake(self.frame.midX, self.frame.midY)
        
        // ShapeNodeをsceneに追加.
        self.addChild(Triangle)
    }
} 
```

## 2.3と3.0の差分
* ```didMoveToView(view: SKView)``` から ```didMove(to view: SKView)``` に変更

## Reference
* SKShapeNode
    * [https://developer.apple.com/reference/spritekit/skshapenode](https://developer.apple.com/reference/spritekit/skshapenode)
