# アルバムを検索する

![Preview photokit003](./img/PhotoKit003.png)

## Swift 3.0

`Info.plist`に`NSPhotoLibraryUsageDescription`を追加します

```swift
//
//  ViewController.swift
//  phptokit003
//
//  Copyright © 2016年 FaBo, Inc. All rights reserved.
//
import UIKit
import Photos

extension PHPhotoLibrary {
    
    //ユーザーに許可を促す.
    class func Authorization(){
        
        PHPhotoLibrary.requestAuthorization { (status) -> Void in
            
            switch(status){
            case .authorized:
                print("Authorized")
            case .denied:
                print("Denied")
            case .notDetermined:
                print("NotDetermined")
            case .restricted:
                print("Restricted")
            }
            
        }
    }
}

class ViewController: UIViewController, UITableViewDataSource {
    
    // アルバム.
    var myAlbum: [String] = []
    
    override func viewDidLoad() {
        
        // フォトアプリの中にあるアルバムを検索する.
        let list = PHAssetCollection.fetchAssetCollections(with: PHAssetCollectionType.album, subtype: PHAssetCollectionSubtype.any, options: nil)
        
        // リストの中にあるオブジェクトに対して１つずつ呼び出す.
        for i in 0 ..< list.count {
            let item = list.object(at: i)
            // アルバムのタイトル名をコレクションする.
            myAlbum.append(item.localizedTitle!)
        }

        // 結果表示用のTableViewを用意.
        let barHeight = UIApplication.shared.statusBarFrame.size.height
        let displayWidth = self.view.frame.width
        let displayHeight = self.view.frame.height
        
        // TableViewを生成.
        let myTableView: UITableView = UITableView(frame: CGRect(x: 0, y: barHeight, width: displayWidth, height: displayHeight - barHeight))
        myTableView.register(UITableViewCell.self, forCellReuseIdentifier: "MyCell")
        myTableView.dataSource = self
        self.view.addSubview(myTableView)
    }
    
    
    // MARK: - UITableViewDataSource
    
    // Cell数を返すメソッド.
    func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
        return myAlbum.count
    }
    
    // Cellの初期化をするメソッド.
    func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
        let cell = tableView.dequeueReusableCell(withIdentifier: "MyCell", for: indexPath)
        // Cellに値を設定.
        cell.textLabel?.text = myAlbum[indexPath.row]
        return cell
    }
    
}
```

## Swift 2.3

```swift
//
//  ViewController.swift
//  phptokit003
//
//  Copyright © 2016年 FaBo, Inc. All rights reserved.
//
import UIKit
import Photos

extension PHPhotoLibrary {
    
    //ユーザーに許可を促す.
    class func Authorization(){
        
        PHPhotoLibrary.requestAuthorization { (status) -> Void in
            
            switch(status){
            case .Authorized:
                print("Authorized")
            case .Denied:
                print("Denied")
            case .NotDetermined:
                print("NotDetermined")
            case .Restricted:
                print("Restricted")
            }
            
        }
    }
}

class ViewController: UIViewController, UITableViewDataSource {
    
    // アルバム.
    var myAlbum: [String] = []
    
    override func viewDidLoad() {
        
        // フォトアプリの中にあるアルバムを検索する.
        let list = PHAssetCollection.fetchAssetCollectionsWithType(PHAssetCollectionType.Album, subtype: PHAssetCollectionSubtype.Any, options: nil)
        
        // リストの中にあるオブジェクトに対して１つずつ呼び出す.
        list.enumerateObjectsUsingBlock { (album, index, isStop) in
            // アルバムのタイトル名をコレクションする.
            self.myAlbum.append(album.localizedTitle)
        }
        
        // 結果表示用のTableViewを用意.
        let barHeight = UIApplication.sharedApplication().statusBarFrame.size.height
        let displayWidth = self.view.frame.width
        let displayHeight = self.view.frame.height
        
        // TableViewを生成.
        let myTableView: UITableView = UITableView(frame: CGRect(x: 0, y: barHeight, width: displayWidth, height: displayHeight - barHeight))
        myTableView.registerClass(UITableViewCell.self, forCellReuseIdentifier: "MyCell")
        myTableView.dataSource = self
        self.view.addSubview(myTableView)
        
    }
    
    
    // MARK: - UITableViewDataSource
    
    // Cell数を返すメソッド.
    func tableView(tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
        print(myAlbum.count)
        return myAlbum.count
    }
    
    // Cellの初期化をするメソッド.
    func tableView(tableView: UITableView, cellForRowAtIndexPath indexPath: NSIndexPath) -> UITableViewCell {
        let cell = tableView.dequeueReusableCellWithIdentifier("MyCell", forIndexPath: indexPath)
        // Cellに値を設定.
        cell.textLabel?.text = myAlbum[indexPath.row]
        return cell
    }
    
}
```

## 2.xと3.xの差分

* `func tableView(tableView: UITableView, numberOfRowsInSection section: Int) -> Int`から`func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int`に変更
* ``から`func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell`に変更
* `UITableView.dequeueReusableCellWithIdentifier(String, forIndexPath: NSIndexPath)`から`UITableView.dequeueReusableCell(withIdentifier: String, for: IndexPath)`に変更
* `NSIndexPath`から`IndexPath`に変更
* `PHAssetCollection.fetchAssetCollectionsWithType(PHAssetCollectionType, subtype: PHAssetCollectionSubtype, options: PHFetchOptions?)`から`PHAssetCollection.fetchAssetCollections(with: PHAssetCollectionType, subtype: PHAssetCollectionSubtype, options: PHFetchOptions?)`に変更

## Reference

* Photos Framework
    * https://developer.apple.com/library/ios/documentation/Photos/Reference/Photos_Framework/
