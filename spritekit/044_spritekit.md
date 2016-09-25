# Nodeを特定の時間待機させる

![Preview spritekit044](img/spritekit044.png)

## Swift3.0
### GameScene.swift
```swift
//
//  GameScene.swift
//  SpriteKit044
//
//  Created by Misato Morino on 2016/09/20.
//  Copyright © 2016年 Misato Morino. All rights reserved.
//

import SpriteKit

class GameScene: SKScene {
    
    private var sequenceAction : SKAction!
    private var rect : SKShapeNode!
    
    override func didMove(to view: SKView) {
        
        // 赤い四角形のShapeNodeを作る.
        rect = SKShapeNode(rectOf: CGSize(width: 50.0, height: 50.0))
        rect.fillColor = UIColor.red
        rect.position = CGPoint(x: self.frame.midX, y: self.frame.minY)
        
        // 真ん中に移動、２秒待つ、上に移動、を逐次実行させるアクションを作成.
        sequenceAction = SKAction.sequence([
            SKAction.moveTo(y: self.frame.midY, duration: 1.0),
            SKAction.wait(forDuration: 2.0),
            SKAction.moveTo(y: self.frame.maxY, duration: 1.0)
            ])
        
        // sceneにshapeNodeを追加.
        self.addChild(rect)
    }
    
    /*
     touchを感知したら呼ばれるメソッド.
     */
    override func touchesBegan(_ touches: Set<UITouch>, with event: UIEvent?) {
        
        //タッチすると逐次実行される.
        rect.run(sequenceAction)
    }
}
```

## Swift 2.3
### GameScene.swift
```swift
//
//  GameScene.swift
//  SpriteKit044
//
//  Created by Misato Morino on 2016/09/20.
//  Copyright © 2016年 Misato Morino. All rights reserved.
//

import SpriteKit

class GameScene: SKScene {
    
    private var sequenceAction : SKAction!
    private var rect : SKShapeNode!
    
    override func didMoveToView(view: SKView) {
        
        // 赤い四角形のShapeNodeを作る.
        rect = SKShapeNode(rectOfSize: CGSizeMake(50.0, 50.0))
        rect.fillColor = UIColor.redColor()
        rect.position = CGPointMake(self.frame.midX, self.frame.minY)
        
        // 真ん中に移動、２秒待つ、上に移動、を逐次実行させるアクションを作成.
        sequenceAction = SKAction.sequence([
            SKAction.moveToY(self.frame.midY, duration: 1.0),
            SKAction.waitForDuration(2.0),
            SKAction.moveToY(self.frame.maxY, duration: 1.0)
            ])
        
        // sceneにshapeNodeを追加.
        self.addChild(rect)
    }
    
    /*
     touchを感知したら呼ばれるメソッド.
     */
    override func touchesBegan(touches: Set<UITouch>, withEvent event: UIEvent?) {
        
        //タッチすると逐次実行される.
        rect.runAction(sequenceAction)
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
