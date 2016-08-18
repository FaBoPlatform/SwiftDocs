# JSONデータのパース

![Preview connection011](./img/Connection011.png)

### iOS9からリクエスト先によってはATSを無効化・対処する必要があります

## Swift 3.0

```swift
//
//  ViewController.swift
//  swiftdocs
//
//  Created by Misato Morino on 2016/08/15.
//  Copyright © 2016年 Misato Morino. All rights reserved.
//

import UIKit

class ViewController: UIViewController, URLSessionDelegate {
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        self.view.backgroundColor = UIColor.lightGray
        
        //結果表示用のTextViewを用意
        let myTextView = UITextView(frame: CGRect(x: 10, y: 50, width: self.view.frame.width - 20, height: 500))
        
        myTextView.backgroundColor = UIColor(red: 0.9, green: 0.9, blue: 1, alpha: 1.0)
        myTextView.layer.masksToBounds = true
        myTextView.layer.cornerRadius = 20.0
        myTextView.layer.borderWidth = 1
        myTextView.layer.borderColor = UIColor.black.cgColor
        myTextView.font = UIFont.systemFont(ofSize: CGFloat(20))
        myTextView.textColor = UIColor.black
        myTextView.textAlignment = NSTextAlignment.left
        myTextView.dataDetectorTypes = UIDataDetectorTypes.all
        myTextView.layer.shadowOpacity = 0.5
        myTextView.layer.masksToBounds = false
        myTextView.isEditable = false
        
        self.view.addSubview(myTextView)
        
        // 通信用のConfigを生成.
        let config: URLSessionConfiguration =  URLSessionConfiguration.default
        
        // Sessionを生成.
        let session: URLSession = URLSession(configuration: config, delegate: self, delegateQueue: nil)
        
        // 通信先のURLを生成.
        let url:NSURL = NSURL(string: "http://xxx/json_encode.php")!
        
        // タスクの生成.
        let task: URLSessionDataTask = session.dataTask(with: url as URL, completionHandler: { (data, response, err) -> Void in
            
            if data != nil {
                
                let str = NSString(data: data!, encoding: String.Encoding.utf8.rawValue)
                print(str!)
                DispatchQueue.main.async(execute: {
                    myTextView.text = "\(str!)"
                })
                
                do {
                    // 受け取ったJSONデータをパースする.
                    let json: NSDictionary = try JSONSerialization.jsonObject(with: data!, options: JSONSerialization.ReadingOptions.allowFragments) as! NSDictionary
                    
                    // バックグラウンドだとUIの処理が出来ないので、メインスレッドでUIの処理を行わせる.
                    DispatchQueue.main.async(execute: {
                        
                        // パースしたJSONデータへのアクセス.
                        for _value in json {
                            let key: String = _value.key as! String
                            let value: Int = _value.value as! Int
                            myTextView.text = myTextView.text.appending("\n\(key) : \(value)")
                        }
                        
                    })
                    
                } catch {
                    print("error")
                    print(error)
                }
            }
        })
        
        
        // タスクの実行.
        task.resume()
    }
    override func didReceiveMemoryWarning() {
        super.didReceiveMemoryWarning()
        
    }
}
```

## Swift 2.3

```swift
//
//  ViewController.swift
//  Connection011
//
//  Created by Misato Morino on 2016/08/15.
//  Copyright © 2016年 Misato Morino. All rights reserved.
//

import UIKit

class ViewController: UIViewController, NSURLSessionDelegate {
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        self.view.backgroundColor = UIColor.lightGrayColor()
        
        //結果表示用のTextViewを用意
        let myTextView = UITextView(frame: CGRectMake(10, 50, self.view.frame.width - 20, 500))
        
        myTextView.backgroundColor = UIColor(red: 0.9, green: 0.9, blue: 1, alpha: 1.0)
        myTextView.layer.masksToBounds = true
        myTextView.layer.cornerRadius = 20.0
        myTextView.layer.borderWidth = 1
        myTextView.layer.borderColor = UIColor.blackColor().CGColor
        myTextView.font = UIFont.systemFontOfSize(CGFloat(20))
        myTextView.textColor = UIColor.blackColor()
        myTextView.textAlignment = NSTextAlignment.Left
        myTextView.dataDetectorTypes = UIDataDetectorTypes.All
        myTextView.layer.shadowOpacity = 0.5
        myTextView.layer.masksToBounds = false
        myTextView.editable = false
        
        self.view.addSubview(myTextView)
        
        // 通信用のConfigを生成.
        let config:NSURLSessionConfiguration = NSURLSessionConfiguration.defaultSessionConfiguration()
        
        // Sessionを生成.
        let session:NSURLSession = NSURLSession(configuration: config, delegate: self, delegateQueue: nil)
        
        // 通信先のURLを生成.
        let url:NSURL = NSURL(string: "http://xxx/json_encode.php")!
        
        // タスクの生成.
        let task:NSURLSessionDataTask = session.dataTaskWithURL(url, completionHandler: { (data, response, err) -> Void in
            
            if data != nil {
                
                let str = NSString(data: data!, encoding: NSUTF8StringEncoding)
                dispatch_async(dispatch_get_main_queue(), {
                    myTextView.text = "\(str!)"
                })
                
                do { 
                    // 受け取ったJSONデータをパースする.
                    let json:NSDictionary = try NSJSONSerialization.JSONObjectWithData(data!, options: NSJSONReadingOptions.AllowFragments) as! NSDictionary
                    
                    // バックグラウンドだとUIの処理が出来ないので、メインスレッドでUIの処理を行わせる.
                    dispatch_async(dispatch_get_main_queue(), {
                        
                        // パースしたJSONデータへのアクセス.
                        for value in json {
                            myTextView.text = "\(myTextView.text)\n\(value.key) : \(value.value)"
                        }
                        
                    })
                    
                } catch {
                    print(error)
                }
            }
        })
            
        
        // タスクの実行.
        task.resume()
    }
    override func didReceiveMemoryWarning() {
        super.didReceiveMemoryWarning()
        
    }
}
```

## 2.xと3.xの差分

* NSURLSessionからURLSessionに変更（他も同じく）
* デリゲートメソッドの接頭語が```URLSession``` から ```urlSession``` に変更
* NSJSONSerialization から JSONSerialization に変更
* ```NSJSONSerialization.JSONObjectWithData``` から ```JSONSerialization.jsonObject``` に変更
* [String : AnyObject]のオプショナル型の制限が厳しくなっているのか、Swift3では ```\(value.key) : \(value.value)``` のようにforeachの一時変数を直接使うよりも一度keyとvalueをそれぞれに合った型に取り出した方が安定している

## Reference

* URLSession
	* [https://developer.apple.com/reference/foundation/urlsession](https://developer.apple.com/reference/foundation/urlsession)
* URLSessionConfiguration
    * [https://developer.apple.com/reference/foundation/urlsessionconfiguration](https://developer.apple.com/reference/foundation/urlsessionconfiguration)
* NSURL
    * [https://developer.apple.com/reference/foundation/nsurl](https://developer.apple.com/reference/foundation/nsurl)
* JSONSerialization
    * [https://developer.apple.com/reference/foundation/jsonserialization](https://developer.apple.com/reference/foundation/jsonserialization)
