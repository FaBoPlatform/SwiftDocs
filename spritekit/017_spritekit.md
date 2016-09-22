# 剛体同士の衝突を検知する

![Preview spritekit017](img/spritekit017.png)

## Swift3.0
### GameScene.swift
```swift
//
//  GameScene.swift
//  SpriteKit017
//
//  Created by Misato Morino on 2016/09/20.
//  Copyright © 2016年 Misato Morino. All rights reserved.
//

import SpriteKit

class GameScene: SKScene ,SKPhysicsContactDelegate {
    
    private var RedRect : SKShapeNode!
    private var BlueRect : SKShapeNode!
    
    // 衝突に使うBitMask.
    let redCategory: UInt32 = 0x1 << 0
    let blueCategory: UInt32 = 0x1 << 1
    
    override func didMove(to view: SKView) {
        
        // Sceneに剛体を設定.
        self.physicsBody = SKPhysicsBody(edgeLoopFrom: self.frame)
        
        // 衝突を検知するdelegateをSKSceneに設定.
        self.physicsWorld.contactDelegate = self
        self.name = "Scene"
        
        // SKShapeで赤いRectを生成.
        RedRect = SKShapeNode(rectOf: CGSize(width: 50.0, height: 50.0))
        RedRect.position = CGPoint(x: self.frame.midX, y: self.frame.midY)
        RedRect.fillColor = UIColor.red
        RedRect.name = "RedRect"
        
        // SKShapeで青いRectを生成.
        BlueRect = SKShapeNode(rectOf: CGSize(width: 100.0, height: 50.0))
        BlueRect.position = CGPoint(x: self.frame.midX, y: self.frame.minY + 50)
        BlueRect.fillColor = UIColor.blue
        BlueRect.name = "BlueRect"
        
        // 剛体を生成.
        RedRect.physicsBody = SKPhysicsBody(rectangleOf: RedRect.frame.size)
        BlueRect.physicsBody = SKPhysicsBody(rectangleOf: BlueRect.frame.size)
        
        //　自身のカテゴリーを設定.
        RedRect.physicsBody?.categoryBitMask = redCategory
        BlueRect.physicsBody?.categoryBitMask = blueCategory
        
        //　衝突先のBitMaskを設定.
        RedRect.physicsBody?.contactTestBitMask = blueCategory
        BlueRect.physicsBody?.contactTestBitMask = redCategory
        
        // sceneにNodeを追加.
        self.addChild(RedRect)
        self.addChild(BlueRect)
    }
    
    /*
     衝突が検知されたら呼ばれるメソッド
     */
    func didBegin(_ contact: SKPhysicsContact) {
        
        print("contactBodyA: \(contact.bodyA.node!.name)")
        print("contactBodyB: \(contact.bodyB.node!.name)")
        
    }
}

```

## Swift 2.3
### GameScene.swift
```swift 
//
//  GameScene.swift
//  SpriteKit017
//
//  Created by Misato Morino on 2016/09/20.
//  Copyright © 2016年 Misato Morino. All rights reserved.
//

import SpriteKit

class GameScene: SKScene ,SKPhysicsContactDelegate {
    
    private var RedRect : SKShapeNode!
    private var BlueRect : SKShapeNode!
    
    // 衝突に使うBitMask.
    let redCategory: UInt32 = 0x1 << 0
    let blueCategory: UInt32 = 0x1 << 1
    
    override func didMoveToView(view: SKView) {
        
        // Sceneに剛体を設定.
        self.physicsBody = SKPhysicsBody(edgeLoopFromRect: self.frame)
        
        // 衝突を検知するdelegateをSKSceneに設定.
        self.physicsWorld.contactDelegate = self
        self.name = "Scene"
        
        // SKShapeで赤いRectを生成.
        RedRect = SKShapeNode(rectOfSize: CGSizeMake(50.0, 50.0))
        RedRect.position = CGPointMake(self.frame.midX, self.frame.midY)
        RedRect.fillColor = UIColor.redColor()
        RedRect.name = "RedRect"
        
        // SKShapeで青いRectを生成.
        BlueRect = SKShapeNode(rectOfSize: CGSizeMake(100.0, 50.0))
        BlueRect.position = CGPointMake(self.frame.midX, self.frame.minY + 50)
        BlueRect.fillColor = UIColor.blueColor()
        BlueRect.name = "BlueRect"
        
        // 剛体を生成.
        RedRect.physicsBody = SKPhysicsBody(rectangleOfSize: RedRect.frame.size)
        BlueRect.physicsBody = SKPhysicsBody(rectangleOfSize: BlueRect.frame.size)
        
        //　自身のカテゴリーを設定.
        RedRect.physicsBody?.categoryBitMask = redCategory
        BlueRect.physicsBody?.categoryBitMask = blueCategory
        
        //　衝突先のBitMaskを設定.
        RedRect.physicsBody?.contactTestBitMask = blueCategory
        BlueRect.physicsBody?.contactTestBitMask = redCategory
        
        // sceneにNodeを追加.
        self.addChild(RedRect)
        self.addChild(BlueRect)
    }
    
    /*
     衝突が検知されたら呼ばれるメソッド
     */
    func didBeginContact(contact: SKPhysicsContact) {
        
        print("contactBodyA: \(contact.bodyA.node!.name)")
        print("contactBodyB: \(contact.bodyB.node!.name)")
        
    }
} 
```

## 2.3と3.0の差分
* ```didMoveToView(view: SKView)``` から ```didMove(to view: SKView)``` に変更
* ```didBeginContact(contact: SKPhysicsContact)``` から ```didBegin(_ contact: SKPhysicsContact) ``` に変更

## Reference
* SKScene
    * [https://developer.apple.com/reference/spritekit/skscene](https://developer.apple.com/reference/spritekit/skscene)
* SKShapeNode
    * [https://developer.apple.com/reference/spritekit/skshapenode](https://developer.apple.com/reference/spritekit/skshapenode)
