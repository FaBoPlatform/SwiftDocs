# 複数のアクションを同時実行させる

![Preview spritekit041](img/spritekit041.png)

## Swift3.0
### GameScene.swift
```swift
//
//  GameScene.swift
//  SpriteKit041
//
//  Created by Misato Morino on 2016/09/20.
//  Copyright © 2016年 Misato Morino. All rights reserved.
//

import SpriteKit

extension SKScene{
    
    /*
     度数からラジアンに変換するメソッド
     */
    func DegreeToRadian(Degree : Double!)-> CGFloat{
        return CGFloat(Degree) / CGFloat(180.0 * M_1_PI)
    }
}

class GameScene: SKScene {
    
    private var groupAction : SKAction!
    private var rect : SKShapeNode!
    
    override func didMove(to view: SKView) {
        
        // 相対的に回転するアクションを作る.
        let RotateAction = SKAction.rotate(byAngle: DegreeToRadian(Degree: 360.0), duration: 1.0)
        
        // 指定された座標まで移動するアクションを作る.
        let MoveAction = SKAction.moveTo(y: self.frame.maxY, duration: 1.0)
        
        // 相対的に拡大するアクションを作る.
        let ScaleAction = SKAction.scale(by: 2.0, duration: 1.0)
        
        // 同時実行するグループアクションを作る.
        groupAction = SKAction.group([
            RotateAction,
            MoveAction,
            ScaleAction
            ])
        
        // 赤い四角形のshapeNodeを作る.
        rect = SKShapeNode(rectOf: CGSize(width: 50.0, height: 50.0))
        rect.fillColor = UIColor.red
        rect.position = CGPoint(x: self.frame.midX, y: self.frame.midY)
        
        // sceneにshapeNodeを追加.
        self.addChild(rect)
    }
    
    /*
     touchを感知したときに呼ばれるメソッド
     */
    override func touchesBegan(_ touches: Set<UITouch>, with event: UIEvent?) {
        
        // アクションが実行されていないなら.
        if rect.hasActions() == false {
            
            // アクションを実行させる.
            rect.run(groupAction)
        }
    }
}
```

## Swift 2.3
### GameScene.swift
```swift
//
//  GameScene.swift
//  SpriteKit041
//
//  Created by Misato Morino on 2016/09/20.
//  Copyright © 2016年 Misato Morino. All rights reserved.
//

import SpriteKit

extension SKScene{
    
    /*
     度数からラジアンに変換するメソッド
     */
    func DegreeToRadian(Degree : Double!)-> CGFloat{
        return CGFloat(Degree) / CGFloat(180.0 * M_1_PI)
    }
}

class GameScene: SKScene {
    
    private var groupAction : SKAction!
    private var rect : SKShapeNode!
    
    override func didMoveToView(view: SKView) {
        
        // 相対的に回転するアクションを作る.
        let RotateAction = SKAction.rotateByAngle(DegreeToRadian(360.0), duration: 1.0)
        
        // 指定された座標まで移動するアクションを作る.
        let MoveAction = SKAction.moveToY(self.frame.maxY, duration: 1.0)
        
        // 相対的に拡大するアクションを作る.
        let ScaleAction = SKAction.scaleBy(2.0, duration: 1.0)
        
        // 同時実行するグループアクションを作る.
        groupAction = SKAction.group([
            RotateAction,
            MoveAction,
            ScaleAction
            ])
        
        // 赤い四角形のshapeNodeを作る.
        rect = SKShapeNode(rectOfSize: CGSizeMake(50.0, 50.0))
        rect.fillColor = UIColor.redColor()
        rect.position = CGPointMake(self.frame.midX, self.frame.midY)
        
        // sceneにshapeNodeを追加.
        self.addChild(rect)
    }
    
    /*
     touchを感知したときに呼ばれるメソッド
     */
    override func touchesBegan(touches: Set<UITouch>, withEvent event: UIEvent?) {
        
        // アクションが実行されていないなら.
        if rect.hasActions() == false {
            
            // アクションを実行させる.
            rect.runAction(groupAction)
        }
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
