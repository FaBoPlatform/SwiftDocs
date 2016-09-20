# Nodeに子を追加する

![Preview spritekit006](img/spritekit006.png)

## Swift3.0
### GameScene.swift
```swift
//
//  GameScene.swift
//  SpriteKit006
//
//  Created by Misato Morino on 2016/09/20.
//  Copyright © 2016年 Misato Morino. All rights reserved.
//

import SpriteKit

/*
 sceneの中心の座標を返す.
 */
extension SKScene{
    
    func GetMid()->CGPoint{
        return CGPoint(x: self.frame.midX, y: self.frame.midY)
    }
    
}

class GameScene: SKScene{
    
    override func didMove(to view: SKView) {
        
        // 赤い四角形を作る.
        let redRect = SKShapeNode(rectOf: CGSize(width: 100, height: 100))
        redRect.fillColor = UIColor.red
        redRect.position = GetMid()
        
        // Nodeに子を追加.
        self.addChild(redRect)
        
    }
    
}
```

## Swift 2.3
### GameScene.swift
```swift
//
//  GameScene.swift
//  SpriteKit006
//
//  Created by Misato Morino on 2016/09/20.
//  Copyright © 2016年 Misato Morino. All rights reserved.
//

import SpriteKit

/*
 sceneの中心の座標を返す.
 */
extension SKScene{
    
    func GetMid()->CGPoint{
        return CGPointMake(self.frame.midX, self.frame.midY)
    }
    
}

class GameScene: SKScene{
    
    override func didMoveToView(view: SKView) {
        
        // 赤い四角形を作る.
        let redRect = SKShapeNode(rectOfSize: CGSizeMake(100, 100))
        redRect.fillColor = UIColor.redColor()
        redRect.position = GetMid()
        
        // Nodeに子を追加.
        self.addChild(redRect)
        
    }
    
}
```

## 2.3と3.0の差分
* ```didMoveToView(view: SKView)``` から ```didMove(to view: SKView)``` に変更

## Reference
* SKScene
    * [https://developer.apple.com/reference/spritekit/skscene](https://developer.apple.com/reference/spritekit/skscene)
* SKShapeNode
    * [https://developer.apple.com/reference/spritekit/skshapenode](https://developer.apple.com/reference/spritekit/skshapenode)
