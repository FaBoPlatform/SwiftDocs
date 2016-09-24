# 画像を描画する

![Preview spritekit027](img/spritekit027.png)

## Swift3.0
### GameScene.swift
```swift
//
//  GameScene.swift
//  SpriteKit027
//
//  Created by Misato Morino on 2016/09/20.
//  Copyright © 2016年 Misato Morino. All rights reserved.
//

import SpriteKit

class GameScene: SKScene {
    override func didMove(to view: SKView) {
        
        // 画像を生成.
        let myImage = SKSpriteNode(imageNamed: "sample")
        
        // 画像の描画場所を指定.
        myImage.position = CGPoint(x:self.frame.midX, y:self.frame.midY);
        
        // シーンに追加.
        self.addChild(myImage)
    }
    
}
```

## Swift 2.3
### GameScene.swift
```swift 
//
//  GameScene.swift
//  SpriteKit027
//
//  Created by Misato Morino on 2016/09/20.
//  Copyright © 2016年 Misato Morino. All rights reserved.
//

import SpriteKit

class GameScene: SKScene {
    override func didMoveToView(view: SKView) {
        
        // 画像を生成.
        let myImage = SKSpriteNode(imageNamed: "sample")
        
        // 画像の描画場所を指定.
        myImage.position = CGPoint(x:CGRectGetMidX(self.frame), y:CGRectGetMidY(self.frame));
        
        // シーンに追加.
        self.addChild(myImage)
    }
    
}
```

## 2.3と3.0の差分
* ```didMoveToView(view: SKView)``` から ```didMove(to view: SKView)``` に変更

## Reference
* SKSpriteNode
    * [https://developer.apple.com/reference/spritekit/skspritenode](https://developer.apple.com/reference/spritekit/skspritenode)
