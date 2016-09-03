# Bluetooth機器の検出

## Swift 3.0

```swift
//
//  ViewController.swift
//  corebluetooth001
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
    let myButton = UIButton()
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        // Status Barの高さを取得.
        let barHeight = UIApplication.shared.statusBarFrame.size.height
        
        // Viewの高さと幅を取得.
        let displayWidth = self.view.frame.width
        let displayHeight = self.view.frame.height
        
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
        myButton.addTarget(self, action: #selector(onClickMyButton(sender:)), for: .touchUpInside)
        
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
        print("state \(central.state)")
        
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
        print("peripheral.identifier.uuidString: \(peripheral.identifier.uuidString)")
        
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
    
    
    // MARK: - UITableViewDelegate
    
    /*
     Cellが選択された際に呼び出される.
     */
    func tableView(_ tableView: UITableView, didSelectRowAt indexPath: IndexPath) {
        print("Num: \(indexPath.row)")
        print("Uuid: \(myUuids[indexPath.row])")
        print("Name: \(myNames[indexPath.row])")
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
        let cell = UITableViewCell(style: UITableViewCellStyle.subtitle, reuseIdentifier:"MyCell" )
        
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

## Swift 2.3

```swift
//
//  ViewController.swift
//  corebluetooth001
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
    let myButton = UIButton()
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        // Status Barの高さを取得.
        let barHeight = UIApplication.sharedApplication().statusBarFrame.size.height
        
        // Viewの高さと幅を取得.
        let displayWidth = self.view.frame.width
        let displayHeight = self.view.frame.height
        
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
        print("state \(central.state)")
        
        switch central.state {
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
    
    
    // MARK: - UITableViewDelegate
    
    /*
     Cellが選択された際に呼び出される.
     */
    func tableView(tableView: UITableView, didSelectRowAtIndexPath indexPath: NSIndexPath) {
        print("Num: \(indexPath.row)")
        print("Uuid: \(myUuids[indexPath.row])")
        print("Name: \(myNames[indexPath.row])")
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

## 2.xと3.xの差分

* `UIApplication.sharedApplication()`から`UIApplication.shared`に変更
* `UITableView.registerClass(AnyClass?, forCellReuseIdentifier: String)`から`UITableView.register(AnyClass?, forCellReuseIdentifier: String)`に変更
* `func centralManagerDidUpdateState(central: CBCentralManager)`から`func centralManagerDidUpdateState(_ central: CBCentralManager)`に変更
* `CBCentralManager.scanForPeripheralsWithServices([CBUUID]?, options: [String : AnyObject]?)`から`CBCentralManager.scanForPeripherals(withServices: [CBUUID]?, options: [String : AnyObject]?)`に変更
* `CBPeripheral.identifier.UUIDString`から`CBPeripheral.identifier.uuidString`に変更
* `UIFont.systemFontOfSize(CGFloat)`から`UIFont.systemFont(ofSize: CGFloat)`に変更

## Reference

* CoreBluetooth
    * https://developer.apple.com/library/ios/documentation/CoreBluetooth/Reference/CoreBluetooth_Framework/
* UITableViewDataSource
    * https://developer.apple.com/library/ios/documentation/UIKit/Reference/UITableViewDataSource_Protocol/
