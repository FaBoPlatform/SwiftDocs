# 取得データの最大値・最小値を得る

![Preview healthkit001](./img/healthkit002.png) ![Preview healthkit001_2](./img/healthkit002_2.png)

## Swift3.0

`Info.plist`に`NSHealthUpdateUsageDescription`と`NSHealthShareUsageDescription`を追加します

※ Capabilities -> HealthKit をONにしてください。

![Preview healthkit001_3](./img/healthkit001_3.png)

```swift
//
//  ViewController.swift
//  healthkit016
//
//  Copyright © 2016年 FaBo, Inc. All rights reserved.
//
import UIKit
import HealthKit

class ViewController: UIViewController, UITextFieldDelegate{
    // 各インスタンスの生成.
    var myHealthStore = HKHealthStore()
    var myWriteBodyMassField: UITextField!
    var myReadMaxField: UITextField!
    var myReadMinField: UITextField!
    var myReadButton: UIButton!
    var myWriteButton: UIButton!
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        // 入力フィールドを設置.
        myWriteBodyMassField = UITextField(frame: CGRect(x: 0, y: 0, width: 300, height: 30))
        myWriteBodyMassField.placeholder = "体重を入力してください"
        myWriteBodyMassField.delegate = self
        myWriteBodyMassField.borderStyle = UITextBorderStyle.roundedRect
        myWriteBodyMassField.layer.position = CGPoint(x: self.view.bounds.width/2, y: 200);
        self.view.addSubview(myWriteBodyMassField)
        
        // 書き込みボタンを設置.
        myWriteButton = UIButton()
        myWriteButton.frame = CGRect(x: 0, y: 0, width: 300, height: 40)
        myWriteButton.backgroundColor = UIColor.blue
        myWriteButton.layer.masksToBounds = true
        myWriteButton.setTitle("体重の書き込み", for: UIControlState.normal)
        myWriteButton.setTitleColor(UIColor.white, for: UIControlState.normal)
        myWriteButton.setTitleColor(UIColor.black, for: UIControlState.highlighted)
        myWriteButton.layer.cornerRadius = 20.0
        myWriteButton.layer.position = CGPoint(x: self.view.frame.width/2, y:250)
        myWriteButton.tag = 2
        myWriteButton.addTarget(self, action: #selector(ViewController.onClickMyButton(sender:)), for: .touchUpInside)
        self.view.addSubview(myWriteButton)
        
        // 最大値読み込み用フィールドの設置.
        myReadMaxField = UITextField(frame: CGRect(x: 0, y: 0, width: 300, height: 30))
        myReadMaxField.placeholder = "最大値(kg)"
        myReadMaxField.isEnabled = false
        myReadMaxField.delegate = self
        myReadMaxField.borderStyle = UITextBorderStyle.roundedRect
        myReadMaxField.layer.position = CGPoint(x:self.view.bounds.width/2,y:350)
        myReadMaxField.isEnabled = false
        self.view.addSubview(myReadMaxField)
        
        // 最小値読み込み用フィールドの設置.
        myReadMinField = UITextField(frame: CGRect(x: 0, y: 0, width: 300, height: 30))
        myReadMinField.placeholder = "最小値(kg)"
        myReadMinField.isEnabled = false
        myReadMinField.delegate = self
        myReadMinField.borderStyle = UITextBorderStyle.roundedRect
        myReadMinField.layer.position = CGPoint(x:self.view.bounds.width/2,y:400)
        myReadMinField.isEnabled = false
        self.view.addSubview(myReadMinField)
        
        // 読み込みボタンを設置.
        myReadButton = UIButton()
        myReadButton.frame = CGRect(x: 0, y: 0, width: 300, height: 40)
        myReadButton.backgroundColor = UIColor.red
        myReadButton.layer.masksToBounds = true
        myReadButton.setTitle("今までの体重の最大最小", for: UIControlState.normal)
        myReadButton.setTitleColor(UIColor.white, for: UIControlState.normal)
        myReadButton.setTitleColor(UIColor.black, for: UIControlState.highlighted)
        myReadButton.layer.cornerRadius = 20.0
        myReadButton.layer.position = CGPoint(x: self.view.frame.width/2, y:450)
        myReadButton.tag = 1
        myReadButton.addTarget(self, action: #selector(ViewController.onClickMyButton(sender:)), for: .touchUpInside)
        self.view.addSubview(myReadButton)
    }
    
    override func viewDidAppear(_ animated: Bool) {
        super.viewDidAppear(animated)
        // HealthStoreへの許可を申請.
        requestAuthorization()
    }
    
    /*
     ボタンイベント.
     */
    func onClickMyButton(sender: UIButton){
        if(sender.tag == 1){
            readData()
        } else if(sender.tag == 2){
            if let val = Double(myWriteBodyMassField.text!) {
                writeData(bodyMass: val)
            }
        }
    }
    
    /*
     Healthデータへのアクセスを申請.
     */
    private func requestAuthorization(){
        // 読み込みを許可する型.
        let types = Set(arrayLiteral:
            HKObjectType.quantityType(forIdentifier: HKQuantityTypeIdentifier.bodyMass)!
        )
        // HealthStoreへのアクセス承認をおこなう.
        myHealthStore.requestAuthorization(toShare: types, read: types, completion: { (success, error) in
            if let e = error {
                print("Error: \(e.localizedDescription)")
            }
            print(success ? "Success" : "Failure")
        })
    }
    
    /*
     データの読み出し.
     */
    private func readData() {
        // 取得したいデータのタイプを生成.
        let typeOfBodyMass = HKObjectType.quantityType(forIdentifier: HKQuantityTypeIdentifier.bodyMass)
        let calendar = Calendar.init(identifier: Calendar.Identifier.gregorian)
        let now = Date()
        let startDate = calendar.startOfDay(for: now)
        let endDate = calendar.date(byAdding: Calendar.Component.day, value: 1, to: startDate)
        let predicate = HKQuery.predicateForSamples(withStart: startDate, end: endDate, options: [])
        
        // データ読み出し時のオプションを指定(平均値の計算).
        let statsOptions: HKStatisticsOptions = [HKStatisticsOptions.discreteMax, HKStatisticsOptions.discreteMin]
        
        let staticsQuery = HKStatisticsQuery(quantityType: typeOfBodyMass!, quantitySamplePredicate: predicate, options: statsOptions) { (query, result, error) in
            if let e = error {
                print("Error: \(e.localizedDescription)")
                return
            }
            guard let max = result?.maximumQuantity() else {
                print("Error")
                self.myReadMaxField.text = "Data is not found"
                return
            }
            guard let mim = result?.minimumQuantity() else {
                print("Error")
                self.myReadMinField.text = "Data is not found"
                return
            }
            // 取得したサンプルを単位に合わせる.
            DispatchQueue.main.async {
                // 最大値のデータを取得.
                self.myReadMaxField.text = "最大: \(max)"
                // 最小値のデータを取得.
                self.myReadMinField.text = "最小: \(mim)"
            }
        }
        // queryを発行.
        self.myHealthStore.execute(staticsQuery)
    }
    
    /*
     データの書き込み.
     */
    private func writeData(bodyMass: Double){
        // 入力された値をkgとして扱う.
        let kg = HKUnit.gramUnit(with: .kilo)
        // データのタイプを指定.
        let typeOfBodyMass = HKObjectType.quantityType(forIdentifier: HKQuantityTypeIdentifier.bodyMass)
        // 保存用のデータを作成.
        let myBodyMass = HKQuantity(unit: kg, doubleValue: bodyMass)
        // StoreKit保存用データを作成.
        let myBodyMassData = HKQuantitySample(type: typeOfBodyMass!, quantity: myBodyMass, start: Date(), end: Date())
        // HealthStoreにデータを保存.
        myHealthStore.save(myBodyMassData, withCompletion: {success, error in
            if let e = error {
                print("Error: \(e.localizedDescription)")
                return
            }
            print(success ? "Success" : "Failure")
        })
    }
}
```

## Swift2.3

```swift
//
//  ViewController.swift
//  healthkit016
//
//  Copyright © 2016年 FaBo, Inc. All rights reserved.
//
import UIKit
import HealthKit

class ViewController: UIViewController, UITextFieldDelegate {
    // 各インスタンスの生成.
    var myHealthStore = HKHealthStore()
    var myWriteBodyMassField: UITextField!
    var myReadMaxField: UITextField!
    var myReadMinField: UITextField!
    var myReadButton: UIButton!
    var myWriteButton: UIButton!
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        // 入力フィールドを設置.
        myWriteBodyMassField = UITextField(frame: CGRectMake(0,0,300,30))
        myWriteBodyMassField.placeholder = "体重を入力してください"
        myWriteBodyMassField.delegate = self
        myWriteBodyMassField.borderStyle = UITextBorderStyle.RoundedRect
        myWriteBodyMassField.layer.position = CGPoint(x:self.view.bounds.width/2,y:200);
        self.view.addSubview(myWriteBodyMassField)
        
        // 書き込みボタンを設置.
        myWriteButton = UIButton()
        myWriteButton.frame = CGRectMake(0,0,300,40)
        myWriteButton.backgroundColor = UIColor.blueColor();
        myWriteButton.layer.masksToBounds = true
        myWriteButton.setTitle("現在の体重の書き込み", forState: UIControlState.Normal)
        myWriteButton.setTitleColor(UIColor.whiteColor(), forState: UIControlState.Normal)
        myWriteButton.setTitleColor(UIColor.blackColor(), forState: UIControlState.Highlighted)
        myWriteButton.layer.cornerRadius = 20.0
        myWriteButton.layer.position = CGPoint(x: self.view.frame.width/2, y:250)
        myWriteButton.tag = 2
        myWriteButton.addTarget(self, action: #selector(ViewController.onClickMyButton(_:)), forControlEvents: .TouchUpInside)
        self.view.addSubview(myWriteButton)
        
        // 最大値読み込み用フィールドの設置.
        myReadMaxField = UITextField(frame: CGRectMake(0,0,300,30))
        myReadMaxField.placeholder = "最大値(kg)"
        myReadMaxField.enabled = false
        myReadMaxField.delegate = self
        myReadMaxField.borderStyle = UITextBorderStyle.RoundedRect
        myReadMaxField.layer.position = CGPoint(x:self.view.bounds.width/2,y:350)
        self.view.addSubview(myReadMaxField)
        
        // 最小値読み込み用フィールドの設置.
        myReadMinField = UITextField(frame: CGRectMake(0,0,300,30))
        myReadMinField.placeholder = "最小値(kg)"
        myReadMinField.enabled = false
        myReadMinField.delegate = self
        myReadMinField.borderStyle = UITextBorderStyle.RoundedRect
        myReadMinField.layer.position = CGPoint(x:self.view.bounds.width/2,y:400)
        self.view.addSubview(myReadMinField)
        
        // 読み込みボタンを設置.
        myReadButton = UIButton()
        myReadButton.frame = CGRectMake(0,0,300,40)
        myReadButton.backgroundColor = UIColor.redColor();
        myReadButton.layer.masksToBounds = true
        myReadButton.setTitle("今までの体重の最大最小", forState: UIControlState.Normal)
        myReadButton.setTitleColor(UIColor.whiteColor(), forState: UIControlState.Normal)
        myReadButton.setTitleColor(UIColor.blackColor(), forState: UIControlState.Highlighted)
        myReadButton.layer.cornerRadius = 20.0
        myReadButton.layer.position = CGPoint(x: self.view.frame.width/2, y:450)
        myReadButton.tag = 1
        myReadButton.addTarget(self, action: #selector(ViewController.onClickMyButton(_:)), forControlEvents: .TouchUpInside)
        self.view.addSubview(myReadButton)
    }
    
    override func viewDidAppear(animated: Bool) {
        super.viewDidAppear(animated)
        // HealthStoreへの許可を申請.
        requestAuthorization()
    }
    
    /*
     ボタンイベント.
     */
    func onClickMyButton(sender: UIButton){
        if(sender.tag == 1){
            readData()
        } else if(sender.tag == 2){
            if let val = Double(myWriteBodyMassField.text!) {
                writeData(val)
            }
        }
    }
    
    /*
     Healthデータへのアクセスを申請.
     */
    private func requestAuthorization(){
        // 読み込みを許可する型.
        let types = Set(arrayLiteral:
            HKObjectType.quantityTypeForIdentifier(HKQuantityTypeIdentifierBodyMass)!
        )
        
        // HealthStoreへのアクセス承認をおこなう.
        myHealthStore.requestAuthorizationToShareTypes(types, readTypes: types, completion: { (success, error) in
            if let e = error {
                print("Error: \(e.localizedDescription)")
            }
            print(success ? "Success" : "Failure")
        })
    }
    
    /*
     データの読み出し.
     */
    private func readData() {
        // 取得したいデータのタイプを生成.
        let typeOfBodyMass = HKObjectType.quantityTypeForIdentifier(HKQuantityTypeIdentifierBodyMass)
        let calendar = NSCalendar.init(identifier: NSCalendarIdentifierGregorian)
        let now = NSDate()
        let startDate = calendar!.startOfDayForDate(now)
        let endDate = calendar!.dateByAddingUnit(NSCalendarUnit.Day, value: 1, toDate: startDate, options: NSCalendarOptions.MatchFirst)
        let predicate = HKQuery.predicateForSamplesWithStartDate(startDate, endDate: endDate, options: HKQueryOptions.None)
        
        // データ読み出し時のオプションを指定(平均値の計算).
        let statsOptions: HKStatisticsOptions = [HKStatisticsOptions.DiscreteMax, HKStatisticsOptions.DiscreteMin]
        
        let staticsQuery = HKStatisticsQuery(quantityType: typeOfBodyMass!, quantitySamplePredicate: predicate, options: statsOptions) { (query, result, error) in
            if let e = error {
                print("Error: \(e.localizedDescription)")
                return
            }
            guard let max = result?.maximumQuantity() else {
                print("Error")
                self.myReadMaxField.text = "Data is not found"
                return
            }
            guard let mim = result?.minimumQuantity() else {
                print("Error")
                self.myReadMinField.text = "Data is not found"
                return
            }
            // 取得したサンプルを単位に合わせる.
            dispatch_async(dispatch_get_main_queue(),{
                // 最大値のデータを取得.
                self.myReadMaxField.text = "最大: \(max)"
                // 最小値のデータを取得.
                self.myReadMinField.text = "最小: \(mim)"
            })
        }
        // queryを発行.
        self.myHealthStore.executeQuery(staticsQuery)
    }
    
    /*
     データの書き込み.
     */
    private func writeData(bodyMass: Double){
        // 入力された値をkgとして扱う.
        let kg = HKUnit.gramUnitWithMetricPrefix(.Kilo)
        // データのタイプを指定.
        let typeOfBodyMass = HKObjectType.quantityTypeForIdentifier(HKQuantityTypeIdentifierBodyMass)
        // 保存用のデータを作成.
        let myBodyMass = HKQuantity(unit: kg, doubleValue: bodyMass)
        // StoreKit保存用データを作成.
        let myBodyMassData = HKQuantitySample(type: typeOfBodyMass!, quantity: myBodyMass, startDate: NSDate(), endDate: NSDate())
        // HealthStoreにデータを保存.
        myHealthStore.saveObject(myBodyMassData, withCompletion: {success, error in
            if let e = error {
                print("Error: \(e.localizedDescription)")
                return
            }
            print(success ? "Success" : "Failure")
        })
    }
}
```

## 2.xと3.xの差分

* HealthKitの定数が列挙体に変更
    * 例 : `HKCharacteristicTypeIdentifierDateOfBirth` → `HKCharacteristicTypeIdentifier.dateOfBirth`
* `HKHealthStore.requestAuthorizationToShareTypes(Set<HKSampleType>?, readTypes: Set<HKObjectType>?, completion: (Bool, NSError?) -> Void)`から`HKHealthStore.requestAuthorization(toShare: Set<HKSampleType>?, read: Set<HKObjectType>?, completion: (Bool, Error?) -> Void)`に変更
* `HKObjectType.quantityTypeForIdentifier(String)`から`HKObjectType.quantityType(forIdentifier: HKQuantityTypeIdentifier)`に変更
* `NSDate`から`Date`に変更
* `dispatch_async(dispatch_queue_t, dispatch_block_t)`から`DispatchQueue.main.async(execute: () -> Void)`に変更
* `HKHealthStore.executeQuery(HKQuery)`から`HKHealthStore.execute(HKQuery)`に変更

## References

* HealthKit
    * https://developer.apple.com/reference/healthkit
