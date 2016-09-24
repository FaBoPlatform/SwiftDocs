# 線を描画する

![Preview spritekit022](img/spritekit022.png)

## Swift3.0
### GameScene.swift
```swift
//
//  GameScene.swift
//  SpriteKit022
//
//  Created by Misato Morino on 2016/09/20.
//  Copyright © 2016年 Misato Morino. All rights reserved.
//


import SpriteKit

/*
 sceneの中心の座標を返すメソッド.
 */
extension SKScene{
    
    func GetMid()->CGPoint{
        return CGPoint(x: self.frame.midX, y: self.frame.midY)
    }
    
}

import SpriteKit

class GameScene: SKScene {
    
    override func didMove(to view: SKView) {
        
        // 線を引くための座標を指定. この場合はx方向に150の長さの線.
        var Points = [CGPoint(x: 0.0, y: 0.0), CGPoint(x: 150.0, y: 0.0)]
        
        // 座標から線のShapeNodeを生成.
        let Line = SKShapeNode(points: &Points, count: Points.count)
        
        // ShapeNodeの座標を指定.
        Line.position = GetMid()
        
        // ShapeNodeの線の色を白色に指定.
        Line.strokeColor = UIColor.white
        
        // ShapeNpdeをsceneに追加.
        self.addChild(Line)
        
        // sceneの背景を黒色に設定.
        self.backgroundColor = UIColor.black
    }
    
}
```

## Swift 2.3
### GameScene.swift
```swift 
//
//  GameScene.swift
//  SpriteKit022
//
//  Created by Misato Morino on 2016/09/20.
//  Copyright © 2016年 Misato Morino. All rights reserved.
//

import SpriteKit

/*
 sceneの中心の座標を返すメソッド.
 */
extension SKScene{
    
    func GetMid()->CGPoint{
        return CGPointMake(self.frame.midX, self.frame.midY)
    }
    
}

import SpriteKit

class GameScene: SKScene {
    
    override func didMoveToView(view: SKView) {
        
        // 線を引くための座標を指定. この場合はx方向に150の長さの線.
        var Points = [CGPointMake(0.0, 0.0),CGPointMake(150.0, 0.0)]
        
        // 座標から線のShapeNodeを生成.
        let Line = SKShapeNode(points: &Points, count: Points.count)
        
        // ShapeNodeの座標を指定.
        Line.position = GetMid()
        
        // ShapeNodeの線の色を白色に指定.
        Line.strokeColor = UIColor.whiteColor()
        
        // ShapeNpdeをsceneに追加.
        self.addChild(Line)
        
        // sceneの背景を黒色に設定.
        self.backgroundColor = UIColor.blackColor()
    }
    
}
```

## 2.3と3.0の差分
* ```didMoveToView(view: SKView)``` から ```didMove(to view: SKView)``` に変更

## Reference
* SKShapeNode
    * [https://developer.apple.com/reference/spritekit/skshapenode](https://developer.apple.com/reference/spritekit/skshapenode)
