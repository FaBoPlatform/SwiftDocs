# SKShapeNodeにテクスチャを貼り付ける

![Preview spritekit026](img/spritekit026.png)

## Swift3.0
### GameScene.swift
```swift
//
//  GameScene.swift
//  sprite
//
//  Created by Misato Morino on 2016/09/20.
//  Copyright © 2016年 Misato Morino. All rights reserved.
//

import SpriteKit

class GameScene: SKScene {
    
    override func didMove(to view: SKView) {
        
        // テクスチャを作る.
        let Texture = SKTexture(imageNamed: "sample")
        
        // 白い円を作る.
        let Circle = SKShapeNode(circleOfRadius: 50.0)
        
        Circle.position = CGPoint(x: 0, y: 0)
        Circle.fillColor = UIColor.white
        
        // テクスチャを設定する.
        Circle.fillTexture = Texture
        
        // 赤い楕円を作る.
        let Ellips = SKShapeNode(ellipseOf: CGSize(width: 200.0, height: 150.0))
        Ellips.position = CGPoint(x: 0, y: -Texture.size().height/2)
        Ellips.fillColor = UIColor.red
        
        // テクスチャを設定する.
        Ellips.fillTexture = Texture
        
        // 半透明な四角形を作る.
        let Rect = SKShapeNode(rectOf: CGSize(width: 200.0, height: 150.0))
        Rect.position = CGPoint(x: 0, y: Texture.size().height/2)
        Rect.fillColor = UIColor(red: 1.0, green: 1.0, blue: 1.0, alpha: 0.5)
        
        // テクスチャを設定する.
        Rect.fillTexture = Texture
        
        // spriteを作る.
        let Logo = SKSpriteNode(imageNamed: "sample")
        Logo.position = CGPoint(x: self.size.width/2.0, y: self.size.height/1.0 - Logo.size.height)
        
        self.addChild(Circle)
        self.addChild(Ellips)
        self.addChild(Rect)
        self.addChild(Logo)
        
    } 
} 
```

## Swift 2.3
### GameScene.swift
```swift 
//
//  GameScene.swift
//  SpriteKit026
//
//  Created by Misato Morino on 2016/09/20.
//  Copyright © 2016年 Misato Morino. All rights reserved.
//

import SpriteKit

class GameScene: SKScene {
    
    override func didMoveToView(view: SKView) {
        
        // テクスチャを作る.
        let Texture = SKTexture(imageNamed: "sample")
        
        // 白い円を作る.
        let Circle = SKShapeNode(circleOfRadius: 50.0)
        
        Circle.position = CGPointMake(0, 0)
        Circle.fillColor = UIColor.whiteColor()
        
        // テクスチャを設定する.
        Circle.fillTexture = Texture
        
        // 赤い楕円を作る.
        let Ellips = SKShapeNode(ellipseOfSize: CGSizeMake(200.0, 150.0))
        Ellips.position = CGPointMake(0, -Texture.size().height/2)
        Ellips.fillColor = UIColor.redColor()
        
        // テクスチャを設定する.
        Ellips.fillTexture = Texture
        
        // 半透明な四角形を作る.
        let Rect = SKShapeNode(rectOfSize: CGSizeMake(200.0, 150.0))
        Rect.position = CGPointMake(0, Texture.size().height/2)
        Rect.fillColor = UIColor(red: 1.0, green: 1.0, blue: 1.0, alpha: 0.5)
        
        // テクスチャを設定する.
        Rect.fillTexture = Texture
        
        // spriteを作る.
        let Logo = SKSpriteNode(imageNamed: "sample")
        Logo.position = CGPointMake(self.size.width/2.0,self.size.height/1.0 - Logo.size.height)
        
        self.addChild(Circle)
        self.addChild(Ellips)
        self.addChild(Rect)
        self.addChild(Logo)
        
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
