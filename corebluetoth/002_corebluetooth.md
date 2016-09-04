# Peripheralへの接続とServiceの検索

## Swift 3.0

### AppDelegate.swift

```swift
//
//  AppDelegate.swift
//  corebluetooth002
//
//  Copyright © 2016年 FaBo, Inc. All rights reserved.
//
import UIKit

@UIApplicationMain
class AppDelegate: UIResponder, UIApplicationDelegate {
    
    var window: UIWindow?
    var myViewController: UIViewController?
    
    
    func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [NSObject : AnyObject]?) -> Bool {
        
        //ViewControllerのインスタンス化
        myViewController = ViewController()
        //UINavigationControllerのインスタンス化とrootViewControllerの指定
        let myNavigationController = UINavigationController(rootViewController: myViewController!)
        //UIWindowのインスタンス化
        self.window = UIWindow(frame: UIScreen.main.bounds)
        //UIWindowのrootViewControllerにnavigationControllerを指定
        self.window?.rootViewController = myNavigationController
        //UIWindowの表示
        self.window?.makeKeyAndVisible()
        
        return true
    }
}
```

### ViewController.swift

```swift
//
//  ViewController.swift
//  corebluetooth002
//
//  Copyright © 2016年 FaBo, Inc. All rights reserved.
//
import UIKit
import CoreBluetooth

class ViewController: UIViewController, UITableViewDelegate, UITableViewDataSource, CBCentralManagerDelegate {
    
    var myTableView: UITableView!
    var myUuids: [String] = []
    var myNames: [String] = []
    var myPeripheral: [CBPeripheral] = []
    var myCentralManager: CBCentralManager!
    var myTargetPeripheral: CBPeripheral!
    let myButton: UIButton = UIButton()
    
    let dataSets = NSMutableArray()
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        // Status Barの高さを取得.
        let barHeight = UIApplication.shared.statusBarFrame.size.height
        // Viewの高さと幅を取得.
        let displayWidth: CGFloat = self.view.frame.width
        let displayHeight: CGFloat = self.view.frame.height
        // TableViewの生成( status barの高さ分ずらして表示 ).
        myTableView = UITableView(frame: CGRect(x: 0, y: barHeight, width: displayWidth, height: displayHeight - barHeight))
        // Cellの登録.
        myTableView.register(UITableViewCell.self, forCellReuseIdentifier: "MyCell")
        // DataSourceの設定.
        myTableView.dataSource = self
        // Delegateを設定.
        myTableView.delegate = self
        // Viewに追加する.
        self.view.addSubview(myTableView)
        
        // サイズ
        myButton.frame = CGRect(x: 0, y: 0, width: 200, height: 40)
        myButton.backgroundColor = UIColor.red
        myButton.layer.masksToBounds = true
        myButton.setTitle("検索", for: UIControlState.normal)
        myButton.setTitleColor(UIColor.white, for: UIControlState.normal)
        myButton.layer.cornerRadius = 20.0
        myButton.layer.position = CGPoint(x: self.view.frame.width/2, y:self.view.frame.height-50)
        myButton.tag = 1
        myButton.addTarget(self, action: #selector(ViewController.onClickMyButton(sender:)), for: .touchUpInside)
        
        // UIボタンをViewに追加.
        self.view.addSubview(myButton);
    }
    
    override func didReceiveMemoryWarning() {
        super.didReceiveMemoryWarning()
    }
    
    /*
     ボタンイベント.
     */
    func onClickMyButton(sender: UIButton){
        
        // 配列をリセット.
        myNames = []
        myUuids = []
        myPeripheral = []
        
        // CoreBluetoothを初期化および始動.
        myCentralManager = CBCentralManager(delegate: self, queue: nil, options: nil)
    }
    
    
    // MARK: - CBCentralManagerDelegate
    
    func centralManagerDidUpdateState(_ central: CBCentralManager) {
        print("state \(central.state)");
        switch central.state {
        case .poweredOff:
            print("Bluetoothの電源がOff")
        case .poweredOn:
            print("Bluetoothの電源はOn")
            // BLEデバイスの検出を開始.
            myCentralManager.scanForPeripherals(withServices: nil, options: nil)
        case .resetting:
            print("レスティング状態")
        case .unauthorized:
            print("非認証状態")
        case .unknown:
            print("不明")
        case .unsupported:
            print("非対応")
        }
    }
    
    /*
     BLEデバイスが検出された際に呼び出される.
     */
    func centralManager(_ central: CBCentralManager, didDiscover peripheral: CBPeripheral, advertisementData: [String : AnyObject], rssi RSSI: NSNumber) {
        print("pheripheral.name: \(peripheral.name)")
        print("advertisementData:\(advertisementData)")
        print("RSSI: \(RSSI)")
        print("peripheral.identifier.UUIDString: \(peripheral.identifier.uuidString)")
        
        let kCBAdvDataLocalName = advertisementData["kCBAdvDataLocalName"] as? String
        if let name = kCBAdvDataLocalName {
            myNames.append(name)
        } else {
            myNames.append("no name")
        }
        
        myPeripheral.append(peripheral)
        myUuids.append(peripheral.identifier.uuidString)
        
        myTableView.reloadData()
    }
    
    /*
     Peripheralに接続
     */
    func centralManager(_ central: CBCentralManager, didConnect peripheral: CBPeripheral) {
        print("connect")
        
        // 遷移するViewを定義する.
        let mySecondViewController: SecondViewController = SecondViewController()
        
        print("setPeripheral")
        mySecondViewController.setPeripheral(target: peripheral)
        mySecondViewController.setCentralManager(manager: central)
        mySecondViewController.searchService()
        
        // アニメーションを設定する.
        mySecondViewController.modalTransitionStyle = UIModalTransitionStyle.partialCurl
        
        print(self.navigationController)
        // Viewの移動する.
        self.navigationController?.pushViewController(mySecondViewController, animated: true)
    }
    
    /*
     Peripheralに接続失敗した際
     */
    func centralManager(_ central: CBCentralManager, didFailToConnect peripheral: CBPeripheral, error: Error?) {
        if let e = error {
            print("Error: \(e.localizedDescription)")
            return
        }
        print("not connnect")
    }
    
    
    // MARK: - UITableViewDelegate
    
    /*
     Cellが選択された際に呼び出される.
     */
    func tableView(_ tableView: UITableView, didSelectRowAt indexPath: IndexPath) {
        print("Num: \(indexPath.row)")
        print("Uuid: \(myUuids[indexPath.row])")
        print("Name: \(myNames[indexPath.row])")
        
        self.myTargetPeripheral = myPeripheral[indexPath.row]
        myCentralManager.connect(self.myTargetPeripheral, options: nil)
    }
    
    
    // MARK: - UITableViewDataSource
    
    /*
     Cellの総数を返す.
     */
    func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
        return myUuids.count
    }
    
    /*
     Cellに値を設定する.
     */
    func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
        let cell = UITableViewCell(style: UITableViewCellStyle.subtitle, reuseIdentifier:"MyCell")
        
        // Cellに値を設定.
        cell.textLabel!.sizeToFit()
        cell.textLabel!.textColor = UIColor.red
        cell.textLabel!.text = "\(myNames[indexPath.row])"
        cell.textLabel!.font = UIFont.systemFont(ofSize: 20)
        // Cellに値を設定(下).
        cell.detailTextLabel!.text = "\(myUuids[indexPath.row])"
        cell.detailTextLabel!.font = UIFont.systemFont(ofSize: 12)
        return cell
    }
}
```

### SecondViewController.swift

```swift
//
//  SecondViewController.swift
//  corebluetooth002
//
//  Copyright © 2016年 FaBo, Inc. All rights reserved.
//
import Foundation
import UIKit
import CoreBluetooth

class SecondViewController: UIViewController, UITableViewDelegate, UITableViewDataSource, CBPeripheralDelegate{
    
    var myTableView: UITableView!
    var myServiceUuids: [String] = []
    var myService: [CBService] = []
    var myButtonBefore: UIButton!
    var myTargetPeriperal: CBPeripheral!
    var myCentralManager: CBCentralManager!
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        self.view.backgroundColor = UIColor.blue
        let barHeight: CGFloat = UIApplication.shared.statusBarFrame.size.height
        let displayWidth: CGFloat = self.view.frame.width
        let displayHeight: CGFloat = self.view.frame.height
        // TableViewの生成( status barの高さ分ずらして表示 ).
        myTableView = UITableView(frame: CGRect(x: 0, y: barHeight, width: displayWidth, height: displayHeight - barHeight))
        // Cellの登録.
        myTableView.register(UITableViewCell.self, forCellReuseIdentifier: "MyCell")
        // DataSourceの設定.
        myTableView.dataSource = self
        // Delegateを設定.
        myTableView.delegate = self
        // Viewに追加する.
        self.view.addSubview(myTableView)
    }
    
    override func didMove(toParentViewController parent: UIViewController?) {
        if parent == nil {
            self.myCentralManager.cancelPeripheralConnection(self.myTargetPeriperal)
        }
    }
    
    
    // MARK: - CBPeripheralDelegate
    
    /*
     接続先のPeripheralを設定
     */
    func setPeripheral(target: CBPeripheral) {
        self.myTargetPeriperal = target
        print(target)
    }
    
    /*
     CentralManagerを設定
     */
    func setCentralManager(manager: CBCentralManager) {
        self.myCentralManager = manager
        print(manager)
    }
    
    /*
     Serviceの検索
     */
    func searchService() {
        print("searchService")
        self.myTargetPeriperal.delegate = self
        self.myTargetPeriperal.discoverServices(nil)
    }
    
    func peripheral(_ peripheral: CBPeripheral, didDiscoverServices error: Error?) {
        if let e = error {
            print("Error: \(e.localizedDescription)")
            return
        }
        
        print("didDiscoverServices")
        for service in peripheral.services! {
            myServiceUuids.append(service.uuid.uuidString)
            myService.append(service)
            print("P: \(peripheral.name) - Discovered service S:'\(service.uuid)'")
        }
        
        myTableView.reloadData()
    }
    
    
    // MARK: - UITableViewDelegate
    
    /*
     Cellが選択された際に呼び出される.
     */
    func tableView(_ tableView: UITableView, didSelectRowAt indexPath: IndexPath) {
        print("ServiceUuid: \(myServiceUuids[indexPath.row])")
    }
    
    
    // MARK: - UITableViewDataSource
    
    /*
     Cellの総数を返す.
     */
    func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
        return myServiceUuids.count
    }
    
    /*
     Cellに値を設定する.
     */
    func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
        let cell = UITableViewCell(style: UITableViewCellStyle.subtitle, reuseIdentifier:"MyCell" )
        
        // Cellに値を設定.
        cell.textLabel!.sizeToFit()
        cell.textLabel!.textColor = UIColor.red
        cell.textLabel!.text = "\(myServiceUuids[indexPath.row])"
        cell.textLabel!.font = UIFont.systemFont(ofSize: 16)
        // Cellに値を設定(下).
        cell.detailTextLabel!.text = "Service"
        cell.detailTextLabel!.font = UIFont.systemFont(ofSize: 12)
        
        return cell
    }
}
```

## Swift 2.3

### AppDelegate.swift

```swift
//
//  AppDelegate.swift
//  corebluetooth002
//
//  Copyright © 2016年 FaBo, Inc. All rights reserved.
//
import UIKit

@UIApplicationMain
class AppDelegate: UIResponder, UIApplicationDelegate {

    var window: UIWindow?
    var myViewController: UIViewController?
    
    
    func application(application: UIApplication, didFinishLaunchingWithOptions launchOptions: [NSObject : AnyObject]?) -> Bool {
        
        //ViewControllerのインスタンス化
        myViewController = ViewController()
        //UINavigationControllerのインスタンス化とrootViewControllerの指定
        let myNavigationController = UINavigationController(rootViewController: myViewController!)
        //UIWindowのインスタンス化
        self.window = UIWindow(frame: UIScreen.mainScreen().bounds)
        //UIWindowのrootViewControllerにnavigationControllerを指定
        self.window?.rootViewController = myNavigationController
        //UIWindowの表示
        self.window?.makeKeyAndVisible()
        
        return true
    }
}
```

### ViewController.swift

```swift
//
//  ViewController.swift
//  corebluetooth002
//
//  Copyright © 2016年 FaBo, Inc. All rights reserved.
//
import UIKit
import CoreBluetooth

class ViewController: UIViewController, UITableViewDelegate, UITableViewDataSource, CBCentralManagerDelegate {
    
    var myTableView: UITableView!
    var myUuids: [String] = []
    var myNames: [String] = []
    var myPeripheral: [CBPeripheral] = []
    var myCentralManager: CBCentralManager!
    var myTargetPeripheral: CBPeripheral!
    let myButton: UIButton = UIButton()
    
    let dataSets = NSMutableArray()
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        // Status Barの高さを取得.
        let barHeight = UIApplication.sharedApplication().statusBarFrame.size.height
        // Viewの高さと幅を取得.
        let displayWidth: CGFloat = self.view.frame.width
        let displayHeight: CGFloat = self.view.frame.height
        // TableViewの生成( status barの高さ分ずらして表示 ).
        myTableView = UITableView(frame: CGRect(x: 0, y: barHeight, width: displayWidth, height: displayHeight - barHeight))
        // Cellの登録.
        myTableView.registerClass(UITableViewCell.self, forCellReuseIdentifier: "MyCell")
        // DataSourceの設定.
        myTableView.dataSource = self
        // Delegateを設定.
        myTableView.delegate = self
        // Viewに追加する.
        self.view.addSubview(myTableView)
        
        // サイズ
        myButton.frame = CGRectMake(0,0,200,40)
        myButton.backgroundColor = UIColor.redColor();
        myButton.layer.masksToBounds = true
        myButton.setTitle("検索", forState: UIControlState.Normal)
        myButton.setTitleColor(UIColor.whiteColor(), forState: UIControlState.Normal)
        myButton.layer.cornerRadius = 20.0
        myButton.layer.position = CGPoint(x: self.view.frame.width/2, y:self.view.frame.height-50)
        myButton.tag = 1
        myButton.addTarget(self, action: #selector(ViewController.onClickMyButton(_:)), forControlEvents: .TouchUpInside)
        
        // UIボタンをViewに追加.
        self.view.addSubview(myButton);
    }
    
    override func didReceiveMemoryWarning() {
        super.didReceiveMemoryWarning()
    }
    
    /*
     ボタンイベント.
     */
    func onClickMyButton(sender: UIButton){
        
        // 配列をリセット.
        myNames = []
        myUuids = []
        myPeripheral = []
        
        // CoreBluetoothを初期化および始動.
        myCentralManager = CBCentralManager(delegate: self, queue: nil, options: nil)
    }
    
    
    // MARK: - CBCentralManagerDelegate
    
    func centralManagerDidUpdateState(central: CBCentralManager) {
        print("state \(central.state)");
        switch (central.state) {
        case .PoweredOff:
            print("Bluetoothの電源がOff")
        case .PoweredOn:
            print("Bluetoothの電源はOn")
            // BLEデバイスの検出を開始.
            myCentralManager.scanForPeripheralsWithServices(nil, options: nil)
        case .Resetting:
            print("レスティング状態")
        case .Unauthorized:
            print("非認証状態")
        case .Unknown:
            print("不明")
        case .Unsupported:
            print("非対応")
        }
    }
    
    /*
     BLEデバイスが検出された際に呼び出される.
     */
    func centralManager(central: CBCentralManager, didDiscoverPeripheral peripheral: CBPeripheral, advertisementData: [String : AnyObject], RSSI: NSNumber) {
        print("pheripheral.name: \(peripheral.name)")
        print("advertisementData:\(advertisementData)")
        print("RSSI: \(RSSI)")
        print("peripheral.identifier.UUIDString: \(peripheral.identifier.UUIDString)")
        
        let kCBAdvDataLocalName = advertisementData["kCBAdvDataLocalName"] as? String
        if let name = kCBAdvDataLocalName {
            myNames.append(name)
        } else {
            myNames.append("no name")
        }
        
        myPeripheral.append(peripheral)
        myUuids.append(peripheral.identifier.UUIDString)
        
        myTableView.reloadData()
    }
    
    /*
     Peripheralに接続
     */
    func centralManager(central: CBCentralManager, didConnectPeripheral peripheral: CBPeripheral) {
        print("connect")
        
        // 遷移するViewを定義する.
        let mySecondViewController: SecondViewController = SecondViewController()
        
        print("setPeripheral")
        mySecondViewController.setPeripheral(peripheral)
        mySecondViewController.setCentralManager(central)
        mySecondViewController.searchService()
        
        // アニメーションを設定する.
        mySecondViewController.modalTransitionStyle = UIModalTransitionStyle.PartialCurl
        
        print(self.navigationController)
        // Viewの移動する.
        self.navigationController?.pushViewController(mySecondViewController, animated: true)
    }
    
    /*
     Peripheralに接続失敗した際
     */
    func centralManager(central: CBCentralManager, didFailToConnectPeripheral peripheral: CBPeripheral, error: NSError?) {
        if let e = error {
            print("Error: \(e.localizedDescription)")
            return
        }
        print("not connnect")
    }
    
    
    // MARK: - UITableViewDelegate
    
    /*
     Cellが選択された際に呼び出される.
     */
    func tableView(tableView: UITableView, didSelectRowAtIndexPath indexPath: NSIndexPath) {
        print("Num: \(indexPath.row)")
        print("Uuid: \(myUuids[indexPath.row])")
        print("Name: \(myNames[indexPath.row])")
        
        self.myTargetPeripheral = myPeripheral[indexPath.row] 
        myCentralManager.connectPeripheral(self.myTargetPeripheral, options: nil)
    }
    
    
    // MARK: - UITableViewDataSource
    
    /*
     Cellの総数を返す.
     */
    func tableView(tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
        return myUuids.count
    }
    
    /*
     Cellに値を設定する.
     */
    func tableView(tableView: UITableView, cellForRowAtIndexPath indexPath: NSIndexPath) -> UITableViewCell {
        let cell = UITableViewCell(style: UITableViewCellStyle.Subtitle, reuseIdentifier:"MyCell" )
        
        // Cellに値を設定.
        cell.textLabel!.sizeToFit()
        cell.textLabel!.textColor = UIColor.redColor()
        cell.textLabel!.text = "\(myNames[indexPath.row])"
        cell.textLabel!.font = UIFont.systemFontOfSize(20)
        // Cellに値を設定(下).
        cell.detailTextLabel!.text = "\(myUuids[indexPath.row])"
        cell.detailTextLabel!.font = UIFont.systemFontOfSize(12)
        return cell
    }
}
```

### SecondViewController.swift

```swift
//
//  SecondViewController.swift
//  corebluetooth002
//
//  Copyright © 2016年 FaBo, Inc. All rights reserved.
//
import Foundation
import UIKit
import CoreBluetooth

class SecondViewController: UIViewController, UITableViewDelegate, UITableViewDataSource, CBPeripheralDelegate{
    
    var myTableView: UITableView!
    var myServiceUuids: [String] = []
    var myService: [CBService] = []
    var myButtonBefore: UIButton!
    var myTargetPeriperal: CBPeripheral!
    var myCentralManager: CBCentralManager!
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        self.view.backgroundColor = UIColor.blueColor()
        let barHeight: CGFloat = UIApplication.sharedApplication().statusBarFrame.size.height
        let displayWidth: CGFloat = self.view.frame.width
        let displayHeight: CGFloat = self.view.frame.height
        // TableViewの生成( status barの高さ分ずらして表示 ).
        myTableView = UITableView(frame: CGRect(x: 0, y: barHeight, width: displayWidth, height: displayHeight - barHeight))
        // Cellの登録.
        myTableView.registerClass(UITableViewCell.self, forCellReuseIdentifier: "MyCell")
        // DataSourceの設定.
        myTableView.dataSource = self
        // Delegateを設定.
        myTableView.delegate = self
        // Viewに追加する.
        self.view.addSubview(myTableView)
    }
    
    override func didMoveToParentViewController(parent: UIViewController?) {
        if let _ = parent {
            self.myCentralManager.cancelPeripheralConnection(self.myTargetPeriperal)
        }
    }
    
    
    // MARK: - CBPeripheralDelegate
    
    /*
     接続先のPeripheralを設定
     */
    func setPeripheral(target: CBPeripheral) {
        self.myTargetPeriperal = target
        print(target)
    }
    
    /*
     CentralManagerを設定
     */
    func setCentralManager(manager: CBCentralManager) {
        self.myCentralManager = manager
        print(manager)
    }
    
    /*
     Serviceの検索
     */
    func searchService() {
        print("searchService")
        self.myTargetPeriperal.delegate = self
        self.myTargetPeriperal.discoverServices(nil)
    }
    
    func peripheral(peripheral: CBPeripheral, didDiscoverServices error: NSError?) {
        if let e = error {
            print("Error: \(e.localizedDescription)")
            return
        }
        
        print("didDiscoverServices")
        for service in peripheral.services! {
            myServiceUuids.append(service.UUID.UUIDString)
            myService.append(service)
            print("P: \(peripheral.name) - Discovered service S:'\(service.UUID)'")
        }
        
        myTableView.reloadData()
    }
    
    
    // MARK: - UITableViewDelegate
    
    /*
     Cellが選択された際に呼び出される.
     */
    func tableView(tableView: UITableView, didSelectRowAtIndexPath indexPath: NSIndexPath) {
        print("ServiceUuid: \(myServiceUuids[indexPath.row])")
    }
    
    
    // MARK: - UITableViewDataSource
    
    /*
     Cellの総数を返す.
     */
    func tableView(tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
        return myServiceUuids.count
    }
    
    /*
     Cellに値を設定する.
     */
    func tableView(tableView: UITableView, cellForRowAtIndexPath indexPath: NSIndexPath) -> UITableViewCell {
        let cell = UITableViewCell(style: UITableViewCellStyle.Subtitle, reuseIdentifier:"MyCell" )
        
        // Cellに値を設定.
        cell.textLabel!.sizeToFit()
        cell.textLabel!.textColor = UIColor.redColor()
        cell.textLabel!.text = "\(myServiceUuids[indexPath.row])"
        cell.textLabel!.font = UIFont.systemFontOfSize(16)
        // Cellに値を設定(下).
        cell.detailTextLabel!.text = "Service"
        cell.detailTextLabel!.font = UIFont.systemFontOfSize(12)
        
        return cell
        
    }
}
```

## 2.xと3.xの差分

* `UIScreen.mainScreen()`から`UIScreen.main`に変更
* `UIApplication.sharedApplication()`から`UIApplication.shared`に変更
* `CBCentralManager.connectPeripheral(CBPeripheral, options: [String : AnyObject]?)`から`CBCentralManager.connect(CBPeripheral, options: [String : AnyObject]?)`に変更
* `func centralManagerDidUpdateState(central: CBCentralManager)`から`func centralManagerDidUpdateState(_ central: CBCentralManager)`に変更
* `CBCentralManager.scanForPeripheralsWithServices([CBUUID]?, options: [String : AnyObject]?)`から`CBCentralManager.scanForPeripherals(withServices: [CBUUID]?, options: [String : AnyObject]?)`に変更
* `CBPeripheral.identifier.UUIDString`から`CBPeripheral.identifier.uuidString`に変更
* `UIFont.systemFontOfSize(CGFloat)`から`UIFont.systemFont(ofSize: CGFloat)`に変更


## Reference

* CoreBluetooth
    * https://developer.apple.com/library/ios/documentation/CoreBluetooth/Reference/CoreBluetooth_Framework/
* UITableViewDataSource
    * https://developer.apple.com/library/ios/documentation/UIKit/Reference/UITableViewDataSource_Protocol/
