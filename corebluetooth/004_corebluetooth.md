# Read/Write/Notify

![Preview coremotion004](./img/CoreBluetooth004_3.png)

### AppDelegate.swift

```swift
//
//  AppDelegate.swift
//  CoreBluetooth004
//
//  Copyright © 2018年 FaBo, Inc. All rights reserved.
//

import UIKit
import CoreData

@UIApplicationMain
class AppDelegate: UIResponder, UIApplicationDelegate {

    var window: UIWindow?


    func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplicationLaunchOptionsKey: Any]?) -> Bool {
        // Override point for customization after application launch.
        //ViewControllerのインスタンス化
        let viewController = ViewController()
        //UINavigationControllerのインスタンス化とrootViewControllerの指定
        let navigationController = UINavigationController(rootViewController: viewController)
        //UIWindowのインスタンス化
        self.window = UIWindow(frame: UIScreen.main.bounds)
        //UIWindowのrootViewControllerにnavigationControllerを指定
        self.window?.rootViewController = navigationController
        //UIWindowの表示
        self.window?.makeKeyAndVisible()
        
        return true
    }

    func applicationWillResignActive(_ application: UIApplication) {
        // Sent when the application is about to move from active to inactive state. This can occur for certain types of temporary interruptions (such as an incoming phone call or SMS message) or when the user quits the application and it begins the transition to the background state.
        // Use this method to pause ongoing tasks, disable timers, and invalidate graphics rendering callbacks. Games should use this method to pause the game.
    }

    func applicationDidEnterBackground(_ application: UIApplication) {
        // Use this method to release shared resources, save user data, invalidate timers, and store enough application state information to restore your application to its current state in case it is terminated later.
        // If your application supports background execution, this method is called instead of applicationWillTerminate: when the user quits.
    }

    func applicationWillEnterForeground(_ application: UIApplication) {
        // Called as part of the transition from the background to the active state; here you can undo many of the changes made on entering the background.
    }

    func applicationDidBecomeActive(_ application: UIApplication) {
        // Restart any tasks that were paused (or not yet started) while the application was inactive. If the application was previously in the background, optionally refresh the user interface.
    }

    func applicationWillTerminate(_ application: UIApplication) {
        // Called when the application is about to terminate. Save data if appropriate. See also applicationDidEnterBackground:.
        // Saves changes in the application's managed object context before the application terminates.
        self.saveContext()
    }

    // MARK: - Core Data stack

    lazy var persistentContainer: NSPersistentContainer = {
        /*
         The persistent container for the application. This implementation
         creates and returns a container, having loaded the store for the
         application to it. This property is optional since there are legitimate
         error conditions that could cause the creation of the store to fail.
        */
        let container = NSPersistentContainer(name: "SwiftBLE")
        container.loadPersistentStores(completionHandler: { (storeDescription, error) in
            if let error = error as NSError? {
                // Replace this implementation with code to handle the error appropriately.
                // fatalError() causes the application to generate a crash log and terminate. You should not use this function in a shipping application, although it may be useful during development.
                 
                /*
                 Typical reasons for an error here include:
                 * The parent directory does not exist, cannot be created, or disallows writing.
                 * The persistent store is not accessible, due to permissions or data protection when the device is locked.
                 * The device is out of space.
                 * The store could not be migrated to the current model version.
                 Check the error message to determine what the actual problem was.
                 */
                fatalError("Unresolved error \(error), \(error.userInfo)")
            }
        })
        return container
    }()

    // MARK: - Core Data Saving support

    func saveContext () {
        let context = persistentContainer.viewContext
        if context.hasChanges {
            do {
                try context.save()
            } catch {
                // Replace this implementation with code to handle the error appropriately.
                // fatalError() causes the application to generate a crash log and terminate. You should not use this function in a shipping application, although it may be useful during development.
                let nserror = error as NSError
                fatalError("Unresolved error \(nserror), \(nserror.userInfo)")
            }
        }
    }

}

```

### ViewController.swift

```swift
//
//  ViewController.swift
//  corebluetooth004
//
//  Copyright © 2018年 FaBo, Inc. All rights reserved.
//
import UIKit
import CoreBluetooth

class ViewController: UIViewController {
    
    var tableView: UITableView!
    var uuids = Array<UUID>()
    var names = [UUID : String]()
    var peripherals = [UUID : CBPeripheral]()
    var targetPeripheral: CBPeripheral!
    var centralManager: CBCentralManager!
    let button = UIButton()
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        // Status Barの高さを取得.
        let barHeight = UIApplication.shared.statusBarFrame.size.height
        
        // Viewの高さと幅を取得.
        let displayWidth = self.view.frame.width
        let displayHeight = self.view.frame.height
        
        // TableViewの生成( status barの高さ分ずらして表示 ).
        tableView = UITableView(frame: CGRect(x: 0, y: barHeight, width: displayWidth, height: displayHeight - barHeight))
        
        // Cellの登録.
        tableView.register(UITableViewCell.self, forCellReuseIdentifier: "MyCell")
        
        // DataSourceの設定.
        tableView.dataSource = self
        
        // Delegateを設定.
        tableView.delegate = self
        
        // Viewに追加する.
        self.view.addSubview(tableView)
        
        // サイズ
        button.frame = CGRect(x: 0, y: 0, width: 200, height: 40)
        button.backgroundColor = UIColor.red
        button.layer.masksToBounds = true
        button.setTitle("検索", for: UIControlState.normal)
        button.setTitleColor(UIColor.white, for: UIControlState.normal)
        button.layer.cornerRadius = 20.0
        button.layer.position = CGPoint(x: self.view.frame.width/2, y:self.view.frame.height-50)
        button.tag = 1
        button.addTarget(self, action: #selector(onClickMyButton(sender:)), for: .touchUpInside)
        
        // UIボタンをViewに追加.
        self.view.addSubview(button);
    }
    
    override func didReceiveMemoryWarning() {
        super.didReceiveMemoryWarning()
    }
    
    /// ボタンが押されたときに呼び出される。
    ///
    /// - Parameter sender: <#sender description#>
    @objc func onClickMyButton(sender: UIButton){
        
        // 配列をリセット.
        self.uuids = []
        self.names = [:]
        self.peripherals = [:]
        
        // CoreBluetoothを初期化および始動.
        centralManager = CBCentralManager(delegate: self, queue: nil, options: nil)
    }
}
extension ViewController: UITableViewDataSource{
    /// Cellの総数を返す。
    ///
    /// - Parameters:
    ///   - tableView: <#tableView description#>
    ///   - section: <#section description#>
    /// - Returns: <#return value description#>
    func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
        return self.names.count
    }
}
extension ViewController: UITableViewDelegate{
    
    /// Cellが選択されたときに呼び出される。
    ///
    /// - Parameters:
    ///   - tableView: <#tableView description#>
    ///   - indexPath: <#indexPath description#>
    func tableView(_ tableView: UITableView, didSelectRowAt indexPath: IndexPath) {
        let uuid = self.uuids[indexPath.row]
        print("Num: \(indexPath.row)")
        print("uuid: \(uuid.description)")
        print("Name: \(String(describing: self.names[uuid]?.description))")
        
        self.targetPeripheral = self.peripherals[uuid]
        self.centralManager.connect(self.targetPeripheral, options: nil)
    }
    
    /// Cellに値を設定する。
    ///
    /// - Parameters:
    ///   - tableView: <#tableView description#>
    ///   - indexPath: <#indexPath description#>
    /// - Returns: <#return value description#>
    func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
        let cell = UITableViewCell(style: UITableViewCellStyle.subtitle, reuseIdentifier:"MyCell" )
        
        let uuid = self.uuids[indexPath.row]
        // Cellに値を設定.
        cell.textLabel!.sizeToFit()
        cell.textLabel!.textColor = UIColor.red
        cell.textLabel!.text = self.names[uuid]
        cell.textLabel!.font = UIFont.systemFont(ofSize: 20)
        // Cellに値を設定(下).
        cell.detailTextLabel!.text = uuid.description
        cell.detailTextLabel!.font = UIFont.systemFont(ofSize: 12)
        return cell
    }
}
extension ViewController: CBCentralManagerDelegate{
    
    /// Central Managerの状態がかわったら呼び出される。
    ///
    /// - Parameter central: Central manager
    func centralManagerDidUpdateState(_ central: CBCentralManager) {
        print("state \(central.state)")
        
        switch central.state {
        case .poweredOff:
            print("Bluetoothの電源がOff")
        case .poweredOn:
            print("Bluetoothの電源はOn")
            // BLEデバイスの検出を開始.
            centralManager.scanForPeripherals(withServices: nil)
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
    
    /// PheripheralのScanが成功したら呼び出される。
    ///
    /// - Parameters:
    ///   - central: <#central description#>
    ///   - peripheral: <#peripheral description#>
    ///   - advertisementData: <#advertisementData description#>
    ///   - RSSI: <#RSSI description#>
    func centralManager(_ central: CBCentralManager, didDiscover peripheral: CBPeripheral,
                        advertisementData: [String: Any], rssi RSSI: NSNumber) {
        print("pheripheral.name: \(String(describing: peripheral.name))")
        print("advertisementData:\(advertisementData)")
        print("RSSI: \(RSSI)")
        print("peripheral.identifier.uuidString: \(peripheral.identifier.uuidString)")
        let uuid = UUID(uuid: peripheral.identifier.uuid)
        self.uuids.append(uuid)
        let kCBAdvDataLocalName = advertisementData["kCBAdvDataLocalName"] as? String
        if let name = kCBAdvDataLocalName {
            self.names[uuid] = name.description
        } else {
            self.names[uuid] = "no name"
        }
        self.peripherals[uuid] = peripheral

        tableView.reloadData()
    }
    
    /// Pheripheralに接続した時に呼ばれる。
    ///
    /// - Parameters:
    ///   - central: <#central description#>
    ///   - peripheral: <#peripheral description#>
    func centralManager(_ central: CBCentralManager, didConnect peripheral: CBPeripheral) {
        print("connect")
        
        // 遷移するViewを定義する.
        let secondViewController: SecondViewController = SecondViewController()
        secondViewController.setPeripheral(target: self.targetPeripheral)
        secondViewController.setCentralManager(manager: self.centralManager)
        secondViewController.searchService()
        
        // アニメーションを設定する.
        secondViewController.modalTransitionStyle = UIModalTransitionStyle.partialCurl
        
        // Viewの移動する.
        self.navigationController?.pushViewController(secondViewController, animated: true)
        
        // Scanを停止する.
        self.centralManager.stopScan()
    }
    
    /// Pheripheralの接続に失敗した時に呼ばれる。
    ///
    /// - Parameters:
    ///   - central: <#central description#>
    ///   - peripheral: <#peripheral description#>
    ///   - error: <#error description#>
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
//  corebluetooth003
//
//  Copyright © 2018年 FaBo, Inc. All rights reserved.
//
import Foundation
import UIKit
import CoreBluetooth

class SecondViewController: UIViewController {
    
    var tableView: UITableView!
    var serviceUuids: [String] = []
    var services: [CBService] = []
    var buttonBefore: UIButton!
    var targetPeriperal: CBPeripheral!
    var centralManager: CBCentralManager!
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        self.view.backgroundColor = UIColor.blue
        let barHeight: CGFloat = UIApplication.shared.statusBarFrame.size.height
        let displayWidth: CGFloat = self.view.frame.width
        let displayHeight: CGFloat = self.view.frame.height
        // TableViewの生成( status barの高さ分ずらして表示 ).
        tableView = UITableView(frame: CGRect(x: 0, y: barHeight, width: displayWidth, height: displayHeight - barHeight))
        // Cellの登録.
        tableView.register(UITableViewCell.self, forCellReuseIdentifier: "MyCell")
        // DataSourceの設定.
        tableView.dataSource = self
        // Delegateを設定.
        tableView.delegate = self
        // Viewに追加する.
        self.view.addSubview(tableView)
    }
    
    override func didMove(toParentViewController parent: UIViewController?) {
        if parent == nil {
            self.centralManager.cancelPeripheralConnection(self.targetPeriperal)
        }
    }
}
extension SecondViewController: UITableViewDelegate{
    
    /// Cellが選択された際に呼び出される.
    ///
    /// - Parameters:
    ///   - tableView: <#tableView description#>
    ///   - indexPath: <#indexPath description#>
    func tableView(_ tableView: UITableView, didSelectRowAt indexPath: IndexPath) {
        print("ServiceUuid: \(serviceUuids[indexPath.row])")
        
        // 遷移するViewを定義する.
        let thirdViewController: ThirdViewController = ThirdViewController()
        thirdViewController.setPeripheral(target: self.targetPeriperal)
        thirdViewController.setService(service: self.services[indexPath.row])
        thirdViewController.searchCharacteristics()
        
        // アニメーションを設定する.
        thirdViewController.modalTransitionStyle = UIModalTransitionStyle.partialCurl
        // Viewの移動する.
        self.navigationController?.pushViewController(thirdViewController, animated: true)
    }
}

extension SecondViewController: UITableViewDataSource{
    
    /// Cellの総数を返す.
    ///
    /// - Parameters:
    ///   - tableView: <#tableView description#>
    ///   - section: <#section description#>
    /// - Returns: <#return value description#>
    func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
        return serviceUuids.count
    }
    
    /// Cellに値を設定する.
    ///
    /// - Parameters:
    ///   - tableView: <#tableView description#>
    ///   - indexPath: <#indexPath description#>
    /// - Returns: <#return value description#>
    func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
        let cell = UITableViewCell(style: UITableViewCellStyle.subtitle, reuseIdentifier:"MyCell" )
        
        // Cellに値を設定.
        cell.textLabel!.sizeToFit()
        cell.textLabel!.textColor = UIColor.red
        cell.textLabel!.text = "\(serviceUuids[indexPath.row])"
        cell.textLabel!.font = UIFont.systemFont(ofSize: 16)
        // Cellに値を設定(下).
        cell.detailTextLabel!.text = "Service"
        cell.detailTextLabel!.font = UIFont.systemFont(ofSize: 12)
        
        return cell
    }
}

extension SecondViewController: CBPeripheralDelegate{
    /// 接続先のPeripheralを設定
    ///
    /// - Parameter target: <#target description#>
    func setPeripheral(target: CBPeripheral) {
        self.targetPeriperal = target
    }
   
    /// CentralManagerを設定
    ///
    /// - Parameter manager: <#manager description#>
    func setCentralManager(manager: CBCentralManager) {
        self.centralManager = manager
    }
    
    /// Serviceの検索
    func searchService() {
        print("searchService")
        self.targetPeriperal.delegate = self
        self.targetPeriperal.discoverServices(nil)
    }

    /// Serviceの検索が終わったら呼び出される
    ///
    /// - Parameters:
    ///   - peripheral: <#peripheral description#>
    ///   - error: <#error description#>
    func peripheral(_ peripheral: CBPeripheral, didDiscoverServices error: Error?) {
        if let e = error {
            print("Error: \(e.localizedDescription)")
            return
        }
        
        print("didDiscoverServices")
        for service in peripheral.services! {
            serviceUuids.append(service.uuid.uuidString)
            services.append(service)
            print("P: \(String(describing: peripheral.name)) - Discovered service S:'\(service.uuid)'")
        }
        
        tableView.reloadData()
    }
    
}

```

### ThirdViewController.swift

```swift
//
//  ThirdViewController.swift
//  corebluetooth003
//
//  Copyright © 2018年 FaBo, Inc. All rights reserved.
//
import Foundation
import UIKit
import CoreBluetooth

class ThirdViewController: UIViewController {
    
    var tableView: UITableView!
    var readTableView: UITableView!
    var services: [CBService] = []
    var characteristics: [CBCharacteristic] = []
    var buttonBefore: UIButton!
    var targetPeriperal: CBPeripheral!
    var targetService: CBService!
    var centralManager: CBCentralManager!
    var targetCharacteristic: CBCharacteristic!
    var readButton: UIButton!
    var notifyLabel: UILabel!
    var writeField: UITextField!
    var writeButton: UIButton!
    var readValues: [Data] = []
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        self.view.backgroundColor = UIColor.cyan
        let barHeight: CGFloat = UIApplication.shared.statusBarFrame.size.height
        let displayWidth: CGFloat = self.view.frame.width
        let displayHeight: CGFloat = self.view.frame.height
        // TableViewの生成( status barの高さ分ずらして表示 ).
        tableView = UITableView(frame: CGRect(x: 0, y: barHeight, width: displayWidth, height: displayHeight/2 - barHeight))
        tableView.register(UITableViewCell.self, forCellReuseIdentifier: "MyCell")
        tableView.dataSource = self
        tableView.delegate = self
        self.view.addSubview(tableView)
        
        readTableView = UITableView(frame: CGRect(x: 0, y: barHeight + displayHeight/2 + 100, width: displayWidth, height: 200))
        readTableView.register(UITableViewCell.self, forCellReuseIdentifier: "MyCell")
        readTableView.dataSource = self
        readTableView.delegate = self
        self.view.addSubview(readTableView)
        
        // Readボタン.
        readButton = UIButton()
        readButton.frame = CGRect(x: displayWidth/2 - 150, y: displayHeight/2+50, width: 100, height: 40)
        readButton.backgroundColor = UIColor.red
        readButton.layer.masksToBounds = true
        readButton.setTitle("Read", for: UIControlState.normal)
        readButton.layer.cornerRadius = 10.0
        readButton.tag = 1
        readButton.addTarget(self, action: #selector(ThirdViewController.onClickMyButton(sender:)), for: .touchUpInside)
        
        // Norifyボタン.
        notifyLabel = UILabel(frame: CGRect(x:displayWidth/2 + 50, y: displayHeight/2+55, width: 100, height: 30))
        notifyLabel.textColor = UIColor.blue
        notifyLabel.text = "Notify"
        notifyLabel.tag = 2
        notifyLabel.isUserInteractionEnabled = true
        
        // UITextField.
        writeField = UITextField(frame: CGRect(x:10, y: displayHeight/2+15, width: displayWidth - 100 - 30, height: 30))
        writeField.text = ""
        writeField.delegate = self
        writeField.borderStyle = .roundedRect
        writeField.clearButtonMode = .whileEditing
        
        // Writeボタン.
        writeButton = UIButton()
        writeButton.frame = CGRect(x: displayWidth - 110, y: displayHeight/2+10, width: 100, height: 40)
        writeButton.backgroundColor = UIColor.blue
        writeButton.layer.masksToBounds = true
        writeButton.setTitle("Write", for: UIControlState.normal)
        writeButton.layer.cornerRadius = 10.0
        writeButton.tag = 2
        writeButton.addTarget(self, action: #selector(ThirdViewController.onClickMyButton(sender:)), for: .touchUpInside)
    }
    override func touchesEnded(_ touches: Set<UITouch>, with event: UIEvent?) {
        for touch: UITouch in touches {
            let tag = touch.view!.tag
            if (tag == 2) {
                if !self.targetCharacteristic.isNotifying {
                    self.targetPeriperal.setNotifyValue(true, for: self.targetCharacteristic)
                    notifyLabel.text = "Stop Notify"
                } else {
                    self.targetPeriperal.setNotifyValue(false, for: self.targetCharacteristic)
                    notifyLabel.text = "Notify"
                }
            }
        }
    }
    
    /// Read, Writeボタンのイベント
    ///
    /// - Parameter sender: <#sender description#>
    @objc func onClickMyButton(sender: UIButton){
        print("onClickMyButton:")
        print("sender.currentTitile: \(String(describing: sender.currentTitle))")
        print("sender.tag:\(sender.tag)")
        
        if let charasteristic = self.targetCharacteristic {
            if(sender.tag == 1){
                self.targetPeriperal.readValue(for: charasteristic)
            }
            else if(sender.tag == 2){
                let data = writeField.text!.data(using: String.Encoding.utf8, allowLossyConversion:true)
                self.targetPeriperal.writeValue(data!, for: targetCharacteristic, type: CBCharacteristicWriteType.withResponse)
            }
        }
    }
    
    /// 各種パーツの追加と削除
    ///
    /// - Parameter characteristic: <#characteristic description#>
    func addButton(characteristic: CBCharacteristic) {
        // Read
        if (self.readButton.isDescendant(of: self.view)) {
            self.readButton.removeFromSuperview()
        }
        if isRead(characteristic: characteristic) {
            self.view.addSubview(self.readButton)
        }
        // Write
        if (self.writeField.isDescendant(of: self.view)) {
            self.writeField.removeFromSuperview()
        }
        if isWrite(characteristic: characteristic) {
            self.view.addSubview(self.writeField)
        }
        if (self.writeButton.isDescendant(of: self.view)) {
            self.writeButton.removeFromSuperview()
        }
        if isWrite(characteristic: characteristic) {
            self.view.addSubview(self.writeButton)
        }
        // Notify
        if (self.notifyLabel.isDescendant(of: self.view)) {
            self.notifyLabel.removeFromSuperview()
        }
        if isNotify(characteristic: characteristic) {
            if !self.targetCharacteristic.isNotifying {
                notifyLabel.text = "Notify"
            } else {
                notifyLabel.text = "Stop Notify"
            }
            self.view.addSubview(self.notifyLabel)
        }
    }
}
extension ThirdViewController: UITextFieldDelegate{
    /// 改行ボタンが押された時の処理
    ///
    /// - Parameter textField: <#textField description#>
    /// - Returns: <#return value description#>
    func textFieldShouldReturn(_ textField: UITextField) -> Bool {
        // 改行ボタンが押されたらKeyboardを閉じる処理.
        textField.resignFirstResponder()
        return true
    }
}
extension ThirdViewController: UITableViewDelegate{
    
    /// Cellが選択された際に呼び出される.
    ///
    /// - Parameters:
    ///   - tableView: <#tableView description#>
    ///   - indexPath: <#indexPath description#>
    func tableView(_ tableView: UITableView, didSelectRowAt indexPath: IndexPath) {
        if tableView.isEqual(self.tableView) {
            self.targetCharacteristic = characteristics[indexPath.row]
            addButton(characteristic: self.targetCharacteristic)
        }
    }
}
extension ThirdViewController: UITableViewDataSource{
    
    /// Cellの総数を返す.
    ///
    /// - Parameters:
    ///   - tableView: <#tableView description#>
    ///   - section: <#section description#>
    /// - Returns: <#return value description#>
    func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
        if tableView.isEqual(self.tableView) {
            return characteristics.count
        } else if tableView.isEqual(self.readTableView) {
            return readValues.count
        }
        return 0
    }
    
    /// Cellに値を設定する.
    ///
    /// - Parameters:
    ///   - tableView: <#tableView description#>
    ///   - indexPath: <#indexPath description#>
    /// - Returns: <#return value description#>
    func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
        let cell = UITableViewCell(style: UITableViewCellStyle.subtitle, reuseIdentifier:"MyCell" )
        // Cellに値を設定.
        cell.textLabel!.sizeToFit()
        cell.textLabel!.textColor = UIColor.red
        cell.textLabel!.font = UIFont.systemFont(ofSize: 16)
        cell.detailTextLabel!.font = UIFont.systemFont(ofSize: 12)
        
        if tableView.isEqual(self.tableView) {
            let characteristic = characteristics[indexPath.row]
            cell.textLabel!.text = "\(characteristic.uuid)"
            var strProp = ""
            if isRead(characteristic: characteristic) {
                strProp += "Read "
            }
            if isWrite(characteristic: characteristic) {
                strProp += "Write "
            }
            if isNotify(characteristic: characteristic) {
                strProp += "Notifiy"
            }
            cell.detailTextLabel!.text = "\(strProp)"
        }
        else if tableView.isEqual(self.readTableView) {
            let value = self.readValues[indexPath.row]
            cell.textLabel!.text = "\(value.base64EncodedString())"
            let now = Date()
            let locale = Locale(identifier: "ja_JP")
            cell.detailTextLabel!.text = "\(now.description(with: locale))"
        }
        
        return cell
    }
}
extension ThirdViewController: CBPeripheralDelegate{
    
    /// CharastaristicがReadされると呼び出される
    ///
    /// - Parameters:
    ///   - peripheral: <#peripheral description#>
    ///   - characteristic: <#characteristic description#>
    ///   - error: <#error description#>
    func peripheral(_ peripheral: CBPeripheral, didUpdateValueFor characteristic: CBCharacteristic, error: Error?) {
        if let e = error {
            print("Error: \(e.localizedDescription)")
            return
        }
        readValues.insert(characteristic.value!, at: 0)
        if readValues.count > 10 {
            readValues.removeLast()
        }
        readTableView.reloadData()
    }
    
    /// 接続先のPeripheralを設定
    ///
    /// - Parameter target: <#target description#>
    func setPeripheral(target: CBPeripheral) {
        self.targetPeriperal = target
    }
    
    /// <#Description#>
    ///
    /// - Parameter service: <#service description#>
    func setService(service: CBService) {
        self.targetService = service
    }
    
    /// Characteristicの検索
    func searchCharacteristics(){
        print("searchService")
        self.targetPeriperal.delegate = self
        self.targetPeriperal.discoverCharacteristics(nil, for: self.targetService)
    }
    
    /// Characteristicの検索が終わったら呼び出される
    ///
    /// - Parameters:
    ///   - peripheral: <#peripheral description#>
    ///   - service: <#service description#>
    ///   - error: <#error description#>
    func peripheral(_ peripheral: CBPeripheral, didDiscoverCharacteristicsFor service: CBService,
                    error: Error?) {
        print("didDiscoverCharacteristicsForService")
        
        for characteristic in service.characteristics! {
            characteristics.append(characteristic)
        }
        tableView.reloadData()
    }
    
    /// Read可能か
    ///
    /// - Parameter characteristic: <#characteristic description#>
    /// - Returns: <#return value description#>
    func isRead(characteristic: CBCharacteristic) -> Bool{
        if characteristic.properties.contains(.read) {
            return true
        }
        return false
    }
    
    /// Write可能か
    ///
    /// - Parameter characteristic: <#characteristic description#>
    /// - Returns: <#return value description#>
    func isWrite(characteristic: CBCharacteristic) -> Bool{
        if characteristic.properties.contains(.write) || characteristic.properties.contains(.writeWithoutResponse) {
            return true
        }
        return false
    }

    /// Notifyに対応しているか
    ///
    /// - Parameter characteristic: <#characteristic description#>
    /// - Returns: <#return value description#>
    func isNotify(characteristic: CBCharacteristic) -> Bool{
        if characteristic.properties.contains(.notify) {
            return true
        }
        return false
    }
}
```

## Reference

* CoreBluetooth
    * https://developer.apple.com/library/ios/documentation/CoreBluetooth/Reference/CoreBluetooth_Framework/
* UITableViewDataSource
    * https://developer.apple.com/library/ios/documentation/UIKit/Reference/UITableViewDataSource_Protocol/
