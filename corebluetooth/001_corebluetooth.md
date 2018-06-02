# Bluetooth機器の検出

![Preview coremotion002](./img/CoreBluetooth001.png)

## Swift 4.0
```swift
//
//  ViewController.swift
//  corebluetooth001
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
}
```

## Reference

* CoreBluetooth
    * https://developer.apple.com/library/ios/documentation/CoreBluetooth/Reference/CoreBluetooth_Framework/
* UITableViewDataSource
    * https://developer.apple.com/library/ios/documentation/UIKit/Reference/UITableViewDataSource_Protocol/
