# Nodeを特定の座標に向かせ続ける

![Preview spritekit013](img/spritekit013.png)

## Swift3.0
### GameScene.swift
```swift
//
//  GameScene.swift
//  SpriteKit013
//
//  Created by Misato Morino on 2016/09/20.
//  Copyright © 2016年 Misato Morino. All rights reserved.
//

import SpriteKit

extension SKScene{
    
    /*
     度数からラジアンに変換するメソッド.
     */
    func degreeToRadian(degree : Double!) -> CGFloat{
        
        return CGFloat(degree) / CGFloat(180.0 * M_1_PI)
        
    }
}

class GameScene: SKScene{
    
    var point : CGPoint!
    
    override func didMove(to view: SKView) {
        
        // 画面中央の座標を取得.
        point = CGPoint(x: self.frame.midX, y: self.frame.midY)
        
    }
    
    /*
     touchを感知した時に呼ばれるメソッド.
     */
    override func touchesBegan(_ touches: Set<UITouch>, with event: UIEvent?) {
        
        for touch : AnyObject in touches{
            
            // touchした場所を取得.
            let location = touch.location(in: self.view)
            
            // textureを作成.
            let texture = SKTexture(imageNamed: "sample")
            
            // spriteNodeを作成.
            let vector = SKSpriteNode(texture: texture)
            vector.position = location
            vector.setScale(0.1)
            vector.name = "Vector"
            
            // 姿勢へのConstraintsを作成.
            let cons = SKConstraint.orient(to: point,offset: SKRange(constantValue: degreeToRadian(degree: -90)))
            
            // Constraintsを適用.
            vector.constraints = [cons]
            
            // sceneにspriteNodeを追加.
            self.addChild(vector)
            
        }
    }
} 
```

## Swift 2.3
### GameScene.swift
```swift
//
//  GameScene.swift
//  SpriteKit013
//
//  Created by Misato Morino on 2016/09/20.
//  Copyright © 2016年 Misato Morino. All rights reserved.
//

import SpriteKit

extension SKScene{
    
    /*
     度数からラジアンに変換するメソッド.
     */
    func degreeToRadian(degree : Double!) -> CGFloat{
        
        return CGFloat(degree) / CGFloat(180.0 * M_1_PI)
        
    }
}

class GameScene: SKScene{
    
    var point : CGPoint!
    
    override func didMoveToView(view: SKView) {
        
        // 画面中央の座標を取得.
        point = CGPointMake(self.frame.midX, self.frame.midY)
        
    }
    
    /*
     touchを感知した時に呼ばれるメソッド.
     */
    override func touchesBegan(touches: Set<UITouch>, withEvent event: UIEvent?) {
        
        for touch : AnyObject in touches{
            
            // touchした場所を取得.
            let location = touch.locationInNode(self)
            
            // textureを作成.
            let texture = SKTexture(imageNamed: "sample")
            
            // spriteNodeを作成.
            let vector = SKSpriteNode(texture: texture)
            vector.position = location
            vector.setScale(0.1)
            vector.name = "Vector"
            
            // 姿勢へのConstraintsを作成.
            let cons = SKConstraint.orientToPoint(point,offset: SKRange(constantValue: degreeToRadian(-90)))
            
            // Constraintsを適用.
            vector.constraints = [cons]
            
            // sceneにspriteNodeを追加.
            self.addChild(vector)
            
        }
    }
} 
```

## 2.3と3.0の差分
* ```didMoveToView(view: SKView)``` から ```didMove(to view: SKView)``` に変更
* ```orientToPoint(point,offset: SKRange(constantValue: degreeToRadian(-90)))``` から ```orient(to: point,offset: SKRange(constantValue: degreeToRadian(degree: -90)))``` に変更

## Reference
* SKTexture
    * [https://developer.apple.com/reference/spritekit/sktexture](https://developer.apple.com/reference/spritekit/sktexture)
* SKConstraint
    * [https://developer.apple.com/reference/spritekit/skconstraint](https://developer.apple.com/reference/spritekit/skconstraint)
