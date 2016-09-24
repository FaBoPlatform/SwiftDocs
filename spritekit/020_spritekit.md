# 円を描画する

![Preview spritekit020](img/spritekit020.png)

## Swift3.0
### GameScene.swift
```swift
//
//  GameScene.swift
//  SpriteKit020
//
//  Created by Misato Morino on 2016/09/20.
//  Copyright © 2016年 Misato Morino. All rights reserved.
//

import SpriteKit

class GameScene: SKScene {
    
    override func didMove(to view: SKView) {
        
        // ShapeNodeを作成.
        let Circle = SKShapeNode(circleOfRadius: 15)
        
        // ShapeNodeの座標を指定.
        Circle.position = CGPoint(x: 0, y: 0)
        
        // ShapeNodeの塗りつぶしの色を指定.
        Circle.fillColor = UIColor.red
        
        // sceneにShapeNodeを追加.
        self.addChild(Circle)
    }
    
} 
```

## Swift 2.3
### GameScene.swift
```swift 
//
//  GameScene.swift
//  SpriteKit020
//
//  Created by Misato Morino on 2016/09/20.
//  Copyright © 2016年 Misato Morino. All rights reserved.
//

import SpriteKit

class GameScene: SKScene {
    
    override func didMoveToView(view: SKView) {
        
        // ShapeNodeを作成.
        let Circle = SKShapeNode(circleOfRadius: 15)
        
        // ShapeNodeの座標を指定.
        Circle.position = CGPointMake(0, 0)
        
        // ShapeNodeの塗りつぶしの色を指定.
        Circle.fillColor = UIColor.redColor()
        
        // sceneにShapeNodeを追加.
        self.addChild(Circle)
    }
    
} 
```

## 2.3と3.0の差分
* ```didMoveToView(view: SKView)``` から ```didMove(to view: SKView)``` に変更

## Reference
* SKShapeNode
    * [https://developer.apple.com/reference/spritekit/skshapenode](https://developer.apple.com/reference/spritekit/skshapenode)
