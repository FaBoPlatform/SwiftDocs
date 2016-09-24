# 線の色を変える

![Preview spritekit025](img/spritekit025.png)

## Swift3.0
### GameScene.swift
```swift
//
//  GameScene.swift
//  sprite
//
//  Created by Misato Morino on 2016/09/20.
//  Copyright © 2016年 Misato Morino. All rights reserved.
//

import SpriteKit

class GameScene: SKScene {
    
    override func didMove(to view: SKView) {
        
        // 円の形のSKShapeNodeを生成.
        let Circle = SKShapeNode(circleOfRadius: 15.0)
        
        // 線の色を変える.
        Circle.strokeColor = UIColor.blue
        
        // ShapaNodeの座標を指定.
        Circle.position = CGPoint(x: self.frame.midX, y: self.frame.midY)
        
        // ShapaNodeの塗りつぶしの色を白色に指定.
        Circle.fillColor = UIColor.white
        
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
//  SpriteKit025
//
//  Created by Misato Morino on 2016/09/20.
//  Copyright © 2016年 Misato Morino. All rights reserved.
//

import SpriteKit

class GameScene: SKScene {
    
    override func didMoveToView(view: SKView) {
        
        // 円の形のSKShapeNodeを生成.
        let Circle = SKShapeNode(circleOfRadius: 15.0)
        
        // 線の色を変える.
        Circle.strokeColor = UIColor.blueColor()
        
        // ShapaNodeの座標を指定.
        Circle.position = CGPointMake(self.frame.midX,self.frame.midY)
        
        // ShapaNodeの塗りつぶしの色を白色に指定.
        Circle.fillColor = UIColor.whiteColor()
        
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
