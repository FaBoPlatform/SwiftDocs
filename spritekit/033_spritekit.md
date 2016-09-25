# 指定した座標に移動するアクションを作る

![Preview spritekit033](img/spritekit033.png)

## Swift3.0
### GameScene.swift
```swift
//
//  GameScene.swift
//  SpriteKit033
//
//  Created by Misato Morino on 2016/09/20.
//  Copyright © 2016年 Misato Morino. All rights reserved.
//

import SpriteKit

class GameScene: SKScene {
    
    private var moveToAction :SKAction!
    private var rect : SKShapeNode!
    
    override func didMove(to view: SKView) {
        
        // 指定した座標に移動するアクションを生成.
        moveToAction = SKAction.move(to: CGPoint(x: 50, y: 50), duration: 1)
        
        // 赤い四角形のShapeNodeを生成.
        rect = SKShapeNode(rectOf: CGSize(width: 25.0, height: 25.0))
        rect.fillColor = UIColor.red
        rect.position = CGPoint(x: 0, y: 0)
        
        // sceneにShapeNodeを追加.
        self.addChild(rect)
    }
    
    override func touchesBegan(_ touches: Set<UITouch>, with event: UIEvent?) {
        
        // アクションを実行.
        rect.run(moveToAction)
    }
} 
```

## Swift 2.3
### GameScene.swift
```swift
//
//  GameScene.swift
//  SpriteKit033
//
//  Created by Misato Morino on 2016/09/20.
//  Copyright © 2016年 Misato Morino. All rights reserved.
//

import SpriteKit

class GameScene: SKScene {
    
    private var moveToAction :SKAction!
    private var rect : SKShapeNode!
    
    override func didMoveToView(view: SKView) {
        
        // 指定した座標に移動するアクションを生成.
        moveToAction = SKAction.moveTo(CGPointMake(50, 50), duration: 1)
        
        // 赤い四角形のShapeNodeを生成.
        rect = SKShapeNode(rectOfSize: CGSizeMake(25.0, 25.0))
        rect.fillColor = UIColor.redColor()
        rect.position = CGPointMake(self.frame.midX,self.frame.midY)
        
        // sceneにShapeNodeを追加.
        self.addChild(rect)
    }
    
    override func touchesBegan(touches: Set<UITouch>, withEvent event: UIEvent?) {
        
        // アクションを実行.
        rect.runAction(moveToAction)
    }
} 
```

## 2.3と3.0の差分
* ```didMoveToView(view: SKView)``` から ```didMove(to view: SKView)``` に変更
* ```runAction``` から ```run``` に変更

## Reference
* SKShapeNode
    * [https://developer.apple.com/reference/spritekit/skshapenode](https://developer.apple.com/reference/spritekit/skshapenode)
* SKAction
    * [https://developer.apple.com/reference/spritekit/skaction](https://developer.apple.com/reference/spritekit/skaction)
