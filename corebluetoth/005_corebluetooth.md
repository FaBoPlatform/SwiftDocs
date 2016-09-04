# Write

## Swift 3.0

### AppDelegate.swift

```swift
//
//  AppDelegate.swift
//  corebluetooth004
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
//  corebluetooth004
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
        let barHeight: CGFloat = UIApplication.shared.statusBarFrame.size.height
        
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
        myButton.addTarget(self, action: #selector(ViewController.onClickMyButton(sender:)), for: .touchUpInside)
        
        // UIボタンをViewに追加.
        self.view.addSubview(myButton);
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
    
    override func didReceiveMemoryWarning() {
        super.didReceiveMemoryWarning()
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
}
```

### SecondViewController.swift

```swift
//
//  SecondViewController.swift
//  corebluetooth004
//
//  Copyright © 2016年 FaBo, Inc. All rights reserved.
//
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
        let barHeight = UIApplication.shared.statusBarFrame.size.height
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
    func searchService(){
        print("searchService")
        self.myTargetPeriperal.delegate = self
        self.myTargetPeriperal.discoverServices(nil)
    }
    
    func peripheral(_ peripheral: CBPeripheral, didDiscoverServices error: Error?) {
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
        
        // 遷移するViewを定義する.
        let myThirdViewController: ThirdViewController = ThirdViewController()
        myThirdViewController.setPeripheral(target: self.myTargetPeriperal)
        myThirdViewController.setService(service: self.myService[indexPath.row])
        myThirdViewController.searchCharacteristics()
        
        // アニメーションを設定する.
        myThirdViewController.modalTransitionStyle = UIModalTransitionStyle.partialCurl
        print(self.navigationController)
        // Viewの移動する.
        self.navigationController?.pushViewController(myThirdViewController, animated: true)
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

### ThirdViewController.swift

```swift
//
//  ThirdViewController.swift
//  corebluetooth05
//
//  Copyright © 2016年 FaBo, Inc. All rights reserved.
//
import UIKit
import CoreBluetooth

class ThirdViewController: UIViewController, UITableViewDelegate, UITableViewDataSource, CBPeripheralDelegate,  UITextFieldDelegate {
    
    var myTableView: UITableView!
    var myCharacteristicsUuids: [String] = []
    var myCharacteristics: [CBCharacteristic] = []
    var myReadButton: UIButton!
    var myWriteButton: UIButton!
    var myTargetPeriperal: CBPeripheral!
    var myService: CBService!
    var myTextField: UITextField!
    var myTargetCharacteristics: CBCharacteristic!
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        self.view.backgroundColor = UIColor.blue
        let barHeight = UIApplication.shared.statusBarFrame.size.height
        let displayWidth = self.view.frame.width
        let displayHeight = self.view.frame.height
        
        // TableViewの生成( status barの高さ分ずらして表示 ).
        myTableView = UITableView(frame: CGRect(x: 0, y: barHeight, width: displayWidth, height: displayHeight/2 - barHeight))
        // Cellの登録.
        myTableView.register(UITableViewCell.self, forCellReuseIdentifier: "MyCell")
        // DataSourceの設定.
        myTableView.dataSource = self
        // Delegateを設定.
        myTableView.delegate = self
        // Viewに追加する.
        self.view.addSubview(myTableView)
        // 文字列を表示
        myTextField = UITextField(frame: CGRect(x: 25, y: displayHeight/2+10, width: displayWidth-50, height: 30))
        myTextField.delegate = self
        myTextField.borderStyle = UITextBorderStyle.roundedRect
        self.view.addSubview(myTextField)
        // Readボタン.
        myReadButton = UIButton()
        myReadButton.frame = CGRect(x: displayWidth/2+60-100/2, y: displayHeight/2+100, width: 100, height: 40)
        myReadButton.backgroundColor = UIColor.red
        myReadButton.layer.masksToBounds = true
        myReadButton.setTitle("Read", for: UIControlState.normal)
        myReadButton.layer.cornerRadius = 20.0
        myReadButton.tag = 1
        myReadButton.addTarget(self, action: #selector(ThirdViewController.onClickMyButton(sender:)), for: .touchUpInside)
        self.view.addSubview(myReadButton)
        // Writeボタン.
        myWriteButton = UIButton()
        myWriteButton.frame = CGRect(x: displayWidth/2-60-100/2, y: displayHeight/2+100, width: 100, height: 40)
        myWriteButton.backgroundColor = UIColor.green
        myWriteButton.layer.masksToBounds = true
        myWriteButton.setTitle("Write", for: UIControlState.normal)
        myWriteButton.layer.cornerRadius = 20.0
        myWriteButton.tag = 2
        myWriteButton.addTarget(self, action: #selector(ThirdViewController.onClickMyButton(sender:)), for: .touchUpInside)
        self.view.addSubview(myWriteButton)
    }
    
    /*
     ボタンイベント.
     */
    func onClickMyButton(sender: UIButton){
        print("onClickMyButton:")
        print("sender.currentTitile: \(sender.currentTitle)")
        print("sender.tag:\(sender.tag)")
        
        if(self.myTargetCharacteristics != nil){
            if(sender.tag == 1){
                self.myTargetPeriperal.readValue(for: myTargetCharacteristics)
            } else if(sender.tag == 2){
                print("write")
                let data = myTextField.text!.data(using: String.Encoding.utf8, allowLossyConversion:true)
                self.myTargetPeriperal.writeValue(data!, for: myTargetCharacteristics, type: CBCharacteristicWriteType.withResponse)
            }
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
    func setService(service: CBService) {
        self.myService = service
        print(service)
    }
    
    /*
     Charactaristicsの検索
     */
    func searchCharacteristics(){
        print("searchService")
        self.myTargetPeriperal.delegate = self
        self.myTargetPeriperal.discoverCharacteristics(nil, for: self.myService)
    }
    
    func peripheral(_ peripheral: CBPeripheral, didDiscoverCharacteristicsFor service: CBService, error: Error?) {
        print("didDiscoverCharacteristicsForService")
        if let e = error {
            print("Error: \(e.localizedDescription)")
            return
        }
        for characteristics in service.characteristics! {
            myCharacteristicsUuids.append(characteristics.uuid.uuidString)
            myCharacteristics.append(characteristics)
        }
        myTableView.reloadData()
    }
    
    /*
     read
     */
    func peripheral(_ peripheral: CBPeripheral, didUpdateValueFor characteristic: CBCharacteristic, error: Error?) {
        if let e = error {
            print("Error: \(e.localizedDescription)")
            return
        }
        print("\(characteristic.value)")
        myTextField.text = "\(characteristic.value)"
    }
    
    /*
     Write
     */
    func peripheral(_ peripheral: CBPeripheral, didWriteValueFor characteristic: CBCharacteristic, error: Error?) {
        if let e = error {
            print("Error: \(e.localizedDescription)")
            return
        }
        print("Success")
    }
    
    // MARK: - UITableViewDelegate
    
    /*
     Cellが選択された際に呼び出される.
     */
    func tableView(_ tableView: UITableView, didSelectRowAt indexPath: IndexPath) {
        print("CharactaristicsUuid: \(myCharacteristicsUuids[indexPath.row])")
        myTargetCharacteristics = myCharacteristics[indexPath.row]
    }
    
    // MARK: - UITableViewDataSource
    
    /*
     Cellの総数を返す.
     */
    func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
        return myCharacteristicsUuids.count
    }
    
    /*
     Cellに値を設定する.
     */
    func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
        let cell = UITableViewCell(style: UITableViewCellStyle.subtitle, reuseIdentifier:"MyCell" )
        // Cellに値を設定.
        cell.textLabel!.sizeToFit()
        cell.textLabel!.textColor = UIColor.red
        cell.textLabel!.text = "\(myCharacteristicsUuids[indexPath.row])"
        cell.textLabel!.font = UIFont.systemFont(ofSize: 16)
        // Cellに値を設定(下).
        cell.detailTextLabel!.text = "Characteristics"
        cell.detailTextLabel!.font = UIFont.systemFont(ofSize: 12)
        return cell
    }
    
    // MARK: - UITextFieldDelegate
    
    /*
     UITextFieldが編集された直後に呼ばれる.
     */
    func textFieldDidBeginEditing(_ textField: UITextField){
        print("textFieldDidBeginEditing:" + textField.text!)
    }
    
    /*
     UITextFieldが編集終了する直前に呼ばれる.
     */
    func textFieldShouldEndEditing(_ textField: UITextField) -> Bool {
        print("textFieldShouldEndEditing:" + textField.text!)
        return true
    }
    
    /*
     改行ボタンが押された際に呼ばれる.
     */
    func textFieldShouldReturn(_ textField: UITextField) -> Bool {
        textField.resignFirstResponder()
        return true
    }
}
```

## Swift 2.3

### AppDelegate.swift

```swift
//
//  AppDelegate.swift
//  corebluetooth005
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
//  corebluetooth005
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
        let barHeight: CGFloat = UIApplication.sharedApplication().statusBarFrame.size.height
        
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
    
    override func didReceiveMemoryWarning() {
        super.didReceiveMemoryWarning()
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
     Cellが選択された際に呼び出される.
     */
    func tableView(tableView: UITableView, didSelectRowAtIndexPath indexPath: NSIndexPath) {
        print("Num: \(indexPath.row)")
        print("Uuid: \(myUuids[indexPath.row])")
        print("Name: \(myNames[indexPath.row])")
        
        self.myTargetPeripheral = myPeripheral[indexPath.row]
        myCentralManager.connectPeripheral(self.myTargetPeripheral, options: nil)
    }
    
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
}
```

### SecondViewController.swift

```swift
//
//  SecondViewController.swift
//  corebluetooth005
//
//  Copyright © 2016年 FaBo, Inc. All rights reserved.
//
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
        let barHeight = UIApplication.sharedApplication().statusBarFrame.size.height
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
    }
    
    override func didMoveToParentViewController(parent: UIViewController?) {
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
    func searchService(){
        print("searchService")
        self.myTargetPeriperal.delegate = self
        self.myTargetPeriperal.discoverServices(nil)
    }
    
    func peripheral(peripheral: CBPeripheral, didDiscoverServices error: NSError?) {
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
        
        // 遷移するViewを定義する.
        let myThirdViewController: ThirdViewController = ThirdViewController()
        myThirdViewController.setPeripheral(self.myTargetPeriperal)
        myThirdViewController.setService(self.myService[indexPath.row])
        myThirdViewController.searchCharacteristics()
        
        // アニメーションを設定する.
        myThirdViewController.modalTransitionStyle = UIModalTransitionStyle.PartialCurl
        print(self.navigationController)
        // Viewの移動する.
        self.navigationController?.pushViewController(myThirdViewController, animated: true)
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

### ThirdViewController.swift

```swift
//
//  ThirdViewController.swift
//  corebluetooth05
//
//  Copyright © 2016年 FaBo, Inc. All rights reserved.
//
import UIKit
import CoreBluetooth

class ThirdViewController: UIViewController, UITableViewDelegate, UITableViewDataSource, CBPeripheralDelegate,  UITextFieldDelegate {
    
    var myTableView: UITableView!
    var myCharacteristicsUuids: [String] = []
    var myCharacteristics: [CBCharacteristic] = []
    var myReadButton: UIButton!
    var myWriteButton: UIButton!
    var myTargetPeriperal: CBPeripheral!
    var myService: CBService!
    var myTextField: UITextField!
    var myTargetCharacteristics: CBCharacteristic!
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        self.view.backgroundColor = UIColor.blueColor()
        let barHeight = UIApplication.sharedApplication().statusBarFrame.size.height
        let displayWidth = self.view.frame.width
        let displayHeight = self.view.frame.height
        
        // TableViewの生成( status barの高さ分ずらして表示 ).
        myTableView = UITableView(frame: CGRect(x: 0, y: barHeight, width: displayWidth, height: displayHeight/2 - barHeight))
        // Cellの登録.
        myTableView.registerClass(UITableViewCell.self, forCellReuseIdentifier: "MyCell")
        // DataSourceの設定.
        myTableView.dataSource = self
        // Delegateを設定.
        myTableView.delegate = self
        // Viewに追加する.
        self.view.addSubview(myTableView)
        // 文字列を表示
        myTextField = UITextField(frame: CGRectMake(25,displayHeight/2 + 10,displayWidth-50,30))
        myTextField.delegate = self
        myTextField.borderStyle = UITextBorderStyle.RoundedRect
        self.view.addSubview(myTextField)
        // Readボタン.
        myReadButton = UIButton()
        myReadButton.frame = CGRectMake(displayWidth/2 + 60 - 100/2,displayHeight/2 + 100,100,40)
        myReadButton.backgroundColor = UIColor.redColor()
        myReadButton.layer.masksToBounds = true
        myReadButton.setTitle("Read", forState: UIControlState.Normal)
        myReadButton.layer.cornerRadius = 20.0
        myReadButton.tag = 1
        myReadButton.addTarget(self, action: #selector(ThirdViewController.onClickMyButton(_:)), forControlEvents: .TouchUpInside)
        self.view.addSubview(myReadButton)
        // Writeボタン.
        myWriteButton = UIButton()
        myWriteButton.frame = CGRectMake(displayWidth/2 - 60 - 100/2,displayHeight/2 + 100,100,40)
        myWriteButton.backgroundColor = UIColor.greenColor()
        myWriteButton.layer.masksToBounds = true
        myWriteButton.setTitle("Write", forState: UIControlState.Normal)
        myWriteButton.layer.cornerRadius = 20.0
        myWriteButton.tag = 2
        myWriteButton.addTarget(self, action: #selector(ThirdViewController.onClickMyButton(_:)), forControlEvents: .TouchUpInside)
        self.view.addSubview(myWriteButton)
    }
    
    /*
     ボタンイベント.
     */
    func onClickMyButton(sender: UIButton){
        print("onClickMyButton:")
        print("sender.currentTitile: \(sender.currentTitle)")
        print("sender.tag:\(sender.tag)")
        
        if(self.myTargetCharacteristics != nil){
            if(sender.tag == 1){
                self.myTargetPeriperal.readValueForCharacteristic(myTargetCharacteristics)
            } else if(sender.tag == 2){
                print("write")
                let data = myTextField.text!.dataUsingEncoding(NSUTF8StringEncoding, allowLossyConversion:true)
                self.myTargetPeriperal.writeValue(data!, forCharacteristic: myTargetCharacteristics, type: CBCharacteristicWriteType.WithResponse)
            }
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
    func setService(service: CBService) {
        self.myService = service
        print(service)
    }
    
    /*
     Charactaristicsの検索
     */
    func searchCharacteristics(){
        print("searchService")
        self.myTargetPeriperal.delegate = self
        self.myTargetPeriperal.discoverCharacteristics(nil, forService: self.myService)
    }
    
    func peripheral(peripheral: CBPeripheral, didDiscoverCharacteristicsForService service: CBService, error: NSError?) {
        print("didDiscoverCharacteristicsForService")
        if let e = error {
            print("Error: \(e.localizedDescription)")
            return
        }
        for characteristics in service.characteristics! {
            myCharacteristicsUuids.append(characteristics.UUID.UUIDString)
            myCharacteristics.append(characteristics)
        }
        myTableView.reloadData()
    }
    
    /*
     read
     */
    func peripheral(peripheral: CBPeripheral, didUpdateValueForCharacteristic characteristic: CBCharacteristic, error: NSError?) {
        if let e = error {
            print("Error: \(e.localizedDescription)")
            return
        }
        print("\(characteristic.value)")
        myTextField.text = "\(characteristic.value)"
    }
    
    /*
     Write
     */
    func peripheral(peripheral: CBPeripheral, didWriteValueForCharacteristic characteristic: CBCharacteristic, error: NSError?) {
        if let e = error {
            print("Error: \(e.localizedDescription)")
            return
        }
        print("Success")
    }
    
    // MARK: - UITableViewDelegate
    
    /*
     Cellが選択された際に呼び出される.
     */
    func tableView(tableView: UITableView, didSelectRowAtIndexPath indexPath: NSIndexPath) {
        print("CharactaristicsUuid: \(myCharacteristicsUuids[indexPath.row])")
        myTargetCharacteristics = myCharacteristics[indexPath.row] 
    }
    
    // MARK: - UITableViewDataSource
    
    /*
     Cellの総数を返す.
     */
    func tableView(tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
        return myCharacteristicsUuids.count
    }
    
    /*
     Cellに値を設定する.
     */
    func tableView(tableView: UITableView, cellForRowAtIndexPath indexPath: NSIndexPath) -> UITableViewCell {
        let cell = UITableViewCell(style: UITableViewCellStyle.Subtitle, reuseIdentifier:"MyCell" )
        // Cellに値を設定.
        cell.textLabel!.sizeToFit()
        cell.textLabel!.textColor = UIColor.redColor()
        cell.textLabel!.text = "\(myCharacteristicsUuids[indexPath.row])"
        cell.textLabel!.font = UIFont.systemFontOfSize(16)
        // Cellに値を設定(下).
        cell.detailTextLabel!.text = "Characteristics"
        cell.detailTextLabel!.font = UIFont.systemFontOfSize(12)
        return cell
    }
    
    // MARK: - UITextFieldDelegate
    
    /*
     UITextFieldが編集された直後に呼ばれる.
     */
    func textFieldDidBeginEditing(textField: UITextField){
        print("textFieldDidBeginEditing:" + textField.text!)
    }
    
    /*
     UITextFieldが編集終了する直前に呼ばれる.
     */
    func textFieldShouldEndEditing(textField: UITextField) -> Bool {
        print("textFieldShouldEndEditing:" + textField.text!)
        return true
    }
    
    /*
     改行ボタンが押された際に呼ばれる.
     */
    func textFieldShouldReturn(textField: UITextField) -> Bool {
        textField.resignFirstResponder()
        return true
    }
}
```

## 2.xと3.xの差分

* `CBPeripheral.readValueForCharacteristic(CBCharacteristic)`から`CBPeripheral.readValue(for: CBCharacteristic)`に変更
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
