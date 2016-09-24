# 四角形を描画する

![Preview spritekit021](img/spritekit021.png)

## Swift3.0
### GameScene.swift
```swift
//
//  GameScene.swift
//  SpriteKit021
//
//  Created by Misato Morino on 2016/09/20.
//  Copyright © 2016年 Misato Morino. All rights reserved.
//

import SpriteKit

/*
 sceceの中心の座標を返すメソッド
 */
extension SKScene{
    
    func GetMid()->CGPoint{
        return CGPoint(x: self.frame.midX, y: self.frame.midY)
    }
    
}

class GameScene: SKScene {
    
    override func didMove(to view: SKView) {
        
        // 青色の長方形のShapeNodeを作成.
        let blueRect = SKShapeNode(rect: CGRect(x: 0.0, y: 0.0, width: 150.0, height: 50.0))
        
        // 座標を中心に指定(ShapeNodeの左上の座標になる).
        blueRect.position = GetMid()
        
        // 塗りつぶしの色を青色に指定.
        blueRect.fillColor = UIColor.blue
        
        // 赤色の長方形のShapeNodeを作成.
        let redRect = SKShapeNode(rectOf: CGSize(width: 150.0, height: 50.0))
        
        // 座標を中心に指定(ShapeNodeの中心の座標になる).
        redRect.position = GetMid()
        
        // 塗りつぶしの色を赤色に指定.
        redRect.fillColor = UIColor.red
        
        
        // sceneの背景を黒色に指定.
        self.backgroundColor = UIColor.black
        
        // sceneにShapeNodeを追加.
        self.addChild(blueRect)
        self.addChild(redRect)
    }
    
} 
```

## Swift 2.3
### GameScene.swift
```swift 
//
//  GameScene.swift
//  SpriteKit021
//
//  Created by Misato Morino on 2016/09/20.
//  Copyright © 2016年 Misato Morino. All rights reserved.
//

import SpriteKit

/*
 sceceの中心の座標を返すメソッド
 */
extension SKScene{
    
    func GetMid()->CGPoint{
        return CGPointMake(self.frame.midX, self.frame.midY)
    }
    
}

class GameScene: SKScene {
    
    override func didMoveToView(view: SKView) {
        
        // 青色の長方形のShapeNodeを作成.
        let blueRect = SKShapeNode(rect: CGRectMake(0.0, 0.0, 150.0, 50.0))
        
        // 座標を中心に指定(ShapeNodeの左上の座標になる).
        blueRect.position = GetMid()
        
        // 塗りつぶしの色を青色に指定.
        blueRect.fillColor = UIColor.blueColor()
        
        // 赤色の長方形のShapeNodeを作成.
        let redRect = SKShapeNode(rectOfSize: CGSizeMake(150.0,50.0))
        
        // 座標を中心に指定(ShapeNodeの中心の座標になる).
        redRect.position = GetMid()
        
        // 塗りつぶしの色を赤色に指定.
        redRect.fillColor = UIColor.redColor()
        
        
        // sceneの背景を黒色に指定.
        self.backgroundColor = UIColor.blackColor()
        
        // sceneにShapeNodeを追加.
        self.addChild(blueRect)
        self.addChild(redRect)
    }
    
}
```

## 2.3と3.0の差分
* ```didMoveToView(view: SKView)``` から ```didMove(to view: SKView)``` に変更

## Reference
* SKShapeNode
    * [https://developer.apple.com/reference/spritekit/skshapenode](https://developer.apple.com/reference/spritekit/skshapenode)
