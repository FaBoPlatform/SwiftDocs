# Nodeに名前をつける

![Preview spritekit009](img/spritekit009.png)

## Swift3.0
### GameScene.swift
```swift
//
//  GameScene.swift
//  SpriteKit009
//
//  Created by Misato Morino on 2016/09/20.
//  Copyright © 2016年 Misato Morino. All rights reserved.
//

import SpriteKit

class GameScene: SKScene {
    
    var Circle : SKShapeNode!
    var BlueRect : SKShapeNode!
    var RedRect : SKShapeNode!
    
    override func didMove(to view: SKView) {
        
        // sceneに名前を付ける.
        self.name = "Scene"
        
        // 円形のNodeを作成.
        Circle = SKShapeNode(circleOfRadius: 50.0)
        Circle.position = CGPoint(x: self.frame.midX, y: self.frame.midY)
        Circle.fillColor = UIColor.green
        
        // 青い正方形のNodeを作成.
        BlueRect = SKShapeNode(rectOf: CGSize(width: 25.0, height: 25.0))
        BlueRect.position = CGPoint(x: self.frame.midX, y: self.frame.midY + 100)
        BlueRect.zPosition = -1.0
        BlueRect.fillColor = UIColor.blue
        
        // 赤い正方形のNodeを作成.
        RedRect = SKShapeNode(rectOf: CGSize(width: 25.0, height: 50.0))
        RedRect.position = CGPoint(x: self.frame.midX, y: self.frame.midY - 100)
        RedRect.zPosition = 0.0
        RedRect.fillColor = UIColor.red
        
        // Nodeに名前を付ける.
        Circle.name = "Circle"
        BlueRect.name = "BlueRect"
        RedRect.name = "RedRect"
        
        // sceneにNodeを追加.
        self.addChild(BlueRect)
        self.addChild(RedRect)
        self.addChild(Circle)
    }
    
    /*
     タッチを感知したときに呼ばれるメソッド.
     */
    override func touchesBegan(_ touches: Set<UITouch>, with event: UIEvent?) {
        
        for touch : AnyObject in touches {
            
            // タッチ位置の取得.
            let location = touch.location(in: self)
            
            // タッチ位置にあったNode.
            let node : SKNode? = self.atPoint(location)
            
            if node != nil {
                print("Node Name: \(node?.name)")
            }
        }
    }
} 
```

## Swift 2.3
### GameScene.swift
```swift
//
//  GameScene.swift
//  SpriteKit009
//
//  Created by Misato Morino on 2016/09/20.
//  Copyright © 2016年 Misato Morino. All rights reserved.
//

import SpriteKit

class GameScene: SKScene {
    
    var Circle : SKShapeNode!
    var BlueRect : SKShapeNode!
    var RedRect : SKShapeNode!
    
    override func didMoveToView(view: SKView) {
        
        // sceneに名前を付ける.
        self.name = "Scene"
        
        // 円形のNodeを作成.
        Circle = SKShapeNode(circleOfRadius: 50.0)
        Circle.position = CGPointMake(self.frame.midX,self.frame.midY)
        Circle.fillColor = UIColor.greenColor()
        
        // 青い正方形のNodeを作成.
        BlueRect = SKShapeNode(rectOfSize: CGSizeMake(25.0, 25.0))
        BlueRect.position = CGPointMake(self.frame.midX,self.frame.midY + 100)
        BlueRect.zPosition = -1.0
        BlueRect.fillColor = UIColor.blueColor()
        
        // 赤い正方形のNodeを作成.
        RedRect = SKShapeNode(rectOfSize: CGSizeMake(25.0, 50.0))
        RedRect.position = CGPointMake(self.frame.midX,self.frame.midY - 100)
        RedRect.zPosition = 0.0
        RedRect.fillColor = UIColor.redColor()
        
        // Nodeに名前を付ける.
        Circle.name = "Circle"
        BlueRect.name = "BlueRect"
        RedRect.name = "RedRect"
        
        // sceneにNodeを追加.
        self.addChild(BlueRect)
        self.addChild(RedRect)
        self.addChild(Circle)
    }
    
    /*
     タッチを感知したときに呼ばれるメソッド.
     */
    override func touchesBegan(touches: Set<UITouch>, withEvent event: UIEvent?) {
        
        for touch : AnyObject in touches {
            
            // タッチ位置の取得.
            let location = touch.locationInNode(self)
            
            // タッチ位置にあったNode.
            let node : SKNode? = self.nodeAtPoint(location)
            
            if node != nil {
                print("Node Name: \(node?.name)")
            }
        }
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
