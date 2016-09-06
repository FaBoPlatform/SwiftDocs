# Pulse oximeter(IEEE Std 11073-10404 Dev specialization)

![Preview healthkit004](./img/healthkit004.png)

## Swift3.0

`Info.plist`に`NSHealthUpdateUsageDescription`と`NSHealthShareUsageDescription`を追加します

※ Capabilities -> HealthKit をONにしてください。

![Preview healthkit001_3](./img/healthkit001_3.png)

```swift
//
//  ViewController.swift
//  healthkit004
//
//  Copyright © 2016年 FaBo, Inc. All rights reserved.
//
import UIKit
import HealthKit

class ViewController: UIViewController, UITextFieldDelegate{
    // 各インスタンスの生成.
    var myHealthStore = HKHealthStore()
    var myReadSpO2Field: UITextField!
    var myWriteSpO2Field: UITextField!
    var myReadButton: UIButton!
    var myWriteButton: UIButton!
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        // 入力フィールドを設置.
        myWriteSpO2Field = UITextField(frame: CGRect(x: 0, y: 0, width: 300, height: 30))
        myWriteSpO2Field.placeholder = "SpO2(酸素飽和度)を入力してください"
        myWriteSpO2Field.delegate = self
        myWriteSpO2Field.borderStyle = UITextBorderStyle.roundedRect
        myWriteSpO2Field.layer.position = CGPoint(x: self.view.bounds.width/2, y: 200);
        self.view.addSubview(myWriteSpO2Field)
        
        // 書き込みボタンを設置.
        myWriteButton = UIButton()
        myWriteButton.frame = CGRect(x: 0, y: 0, width: 300, height: 40)
        myWriteButton.backgroundColor = UIColor.blue
        myWriteButton.layer.masksToBounds = true
        myWriteButton.setTitle("SpO2(酸素飽和度)の書き込み", for: UIControlState.normal)
        myWriteButton.setTitleColor(UIColor.white, for: UIControlState.normal)
        myWriteButton.setTitleColor(UIColor.black, for: UIControlState.highlighted)
        myWriteButton.layer.cornerRadius = 20.0
        myWriteButton.layer.position = CGPoint(x: self.view.frame.width/2, y:250)
        myWriteButton.tag = 2
        myWriteButton.addTarget(self, action: #selector(ViewController.onClickMyButton(sender:)), for: .touchUpInside)
        self.view.addSubview(myWriteButton)
        
        // 表示フィールドを設置.
        myReadSpO2Field = UITextField(frame: CGRect(x: 0, y: 0, width: 300, height: 30))
        myReadSpO2Field.placeholder = "前回登録のSpO2(酸素飽和度)"
        myReadSpO2Field.isEnabled = false
        myReadSpO2Field.delegate = self
        myReadSpO2Field.borderStyle = UITextBorderStyle.roundedRect
        myReadSpO2Field.layer.position = CGPoint(x:self.view.bounds.width/2,y:350);
        myReadSpO2Field.isEnabled = false
        self.view.addSubview(myReadSpO2Field)
        
        // 読み込みボタンを設置.
        myReadButton = UIButton()
        myReadButton.frame = CGRect(x: 0, y: 0, width: 300, height: 40)
        myReadButton.backgroundColor = UIColor.red
        myReadButton.layer.masksToBounds = true
        myReadButton.setTitle("SpO2(酸素飽和度)の読み込み", for: UIControlState.normal)
        myReadButton.setTitleColor(UIColor.white, for: UIControlState.normal)
        myReadButton.setTitleColor(UIColor.black, for: UIControlState.highlighted)
        myReadButton.layer.cornerRadius = 20.0
        myReadButton.layer.position = CGPoint(x: self.view.frame.width/2, y:400)
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
            if let val = Double(myWriteSpO2Field.text!) {
                writeData(SpO2: val)
            }
        }
    }
    
    /*
     Healthデータへのアクセスを申請.
     */
    private func requestAuthorization(){
        // 読み込みを許可する型.
        let types = Set(arrayLiteral:
            HKObjectType.quantityType(forIdentifier: HKQuantityTypeIdentifier.oxygenSaturation)!
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
        let typeOfSpO2 = HKSampleType.quantityType(forIdentifier: HKQuantityTypeIdentifier.oxygenSaturation)
        let calendar = Calendar.init(identifier: Calendar.Identifier.gregorian)
        let now = Date()
        let startDate = calendar.startOfDay(for: now)
        let endDate = calendar.date(byAdding: Calendar.Component.day, value: 1, to: startDate)
        let predicate = HKQuery.predicateForSamples(withStart: startDate, end: endDate, options: [])
        
        // データ取得時に登録された時間でソートするためのDescriptorを生成.
        let mySortDescriptor = NSSortDescriptor(key:HKSampleSortIdentifierStartDate, ascending: false)
        // 体重データ読み出しのためのqueryを生成.
        let mySampleQuery = HKSampleQuery(sampleType: typeOfSpO2!, predicate: predicate, limit: 1, sortDescriptors: [mySortDescriptor])
        { (sampleQuery, results, error ) -> Void in
            // 一番最近に登録されたデータを取得.
            guard let myRecentSample = results!.first as? HKQuantitySample else {
                print("error")
                self.myReadSpO2Field.text = "Data is not found"
                return
            }
            // 取得したサンプルを単位に合わせる.
            DispatchQueue.main.async {
                self.myReadSpO2Field.text = "\(myRecentSample.quantity)"
            }
        }
        
        // queryを発行.
        self.myHealthStore.execute(mySampleQuery)
        
    }
    /*
     データの書き込み.
     */
    private func writeData(SpO2: Double){
        // 登録用データタイプを生成.
        let typeOfSpO2 = HKObjectType.quantityType(forIdentifier: HKQuantityTypeIdentifier.oxygenSaturation)!
        // 単位とセットのデータを生成.
        let percent = HKUnit.percent()
        let mySpO2 = HKQuantity(unit:percent, doubleValue: SpO2)
        // StoreKit保存用データを作成.
        let mySpO2Data = HKQuantitySample(type: typeOfSpO2, quantity: mySpO2, start: Date(), end: Date())
        // HealthStoreにデータを保存.
        myHealthStore.save(mySpO2Data, withCompletion: {success, error in
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
//  healthkit004
//
//  Copyright © 2016年 FaBo, Inc. All rights reserved.
//
import UIKit
import HealthKit

class ViewController: UIViewController, UITextFieldDelegate{
    // 各インスタンスの生成.
    var myHealthStore = HKHealthStore()
    var myReadSpO2Field: UITextField!
    var myWriteSpO2Field: UITextField!
    var myReadButton: UIButton!
    var myWriteButton: UIButton!
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        // 入力フィールドを設置.
        myWriteSpO2Field = UITextField(frame: CGRectMake(0,0,300,30))
        myWriteSpO2Field.placeholder = "SpO2(酸素飽和度)を入力してください"
        myWriteSpO2Field.delegate = self
        myWriteSpO2Field.borderStyle = UITextBorderStyle.RoundedRect
        myWriteSpO2Field.layer.position = CGPoint(x:self.view.bounds.width/2,y:200);
        self.view.addSubview(myWriteSpO2Field)
        
        // 書き込みボタンを設置.
        myWriteButton = UIButton()
        myWriteButton.frame = CGRectMake(0,0,300,40)
        myWriteButton.backgroundColor = UIColor.blueColor();
        myWriteButton.layer.masksToBounds = true
        myWriteButton.setTitle("SpO2(酸素飽和度)の書き込み", forState: UIControlState.Normal)
        myWriteButton.setTitleColor(UIColor.whiteColor(), forState: UIControlState.Normal)
        myWriteButton.setTitleColor(UIColor.blackColor(), forState: UIControlState.Highlighted)
        myWriteButton.layer.cornerRadius = 20.0
        myWriteButton.layer.position = CGPoint(x: self.view.frame.width/2, y:250)
        myWriteButton.tag = 2
        myWriteButton.addTarget(self, action: #selector(ViewController.onClickMyButton(_:)), forControlEvents: .TouchUpInside)
        self.view.addSubview(myWriteButton)
        
        
        // 表示フィールドを設置.
        myReadSpO2Field = UITextField(frame: CGRectMake(0,0,300,30))
        myReadSpO2Field.placeholder = "前回登録のSpO2(酸素飽和度)"
        myReadSpO2Field.enabled = false
        myReadSpO2Field.delegate = self
        myReadSpO2Field.borderStyle = UITextBorderStyle.RoundedRect
        myReadSpO2Field.layer.position = CGPoint(x:self.view.bounds.width/2,y:350);
        myReadSpO2Field.enabled = false
        self.view.addSubview(myReadSpO2Field)
        
        // 読み込みボタンを設置.
        myReadButton = UIButton()
        myReadButton.frame = CGRectMake(0,0,300,40)
        myReadButton.backgroundColor = UIColor.redColor();
        myReadButton.layer.masksToBounds = true
        myReadButton.setTitle("SpO2(酸素飽和度)の読み込み", forState: UIControlState.Normal)
        myReadButton.setTitleColor(UIColor.whiteColor(), forState: UIControlState.Normal)
        myReadButton.setTitleColor(UIColor.blackColor(), forState: UIControlState.Highlighted)
        myReadButton.layer.cornerRadius = 20.0
        myReadButton.layer.position = CGPoint(x: self.view.frame.width/2, y:400)
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
            if let val = Double(myWriteSpO2Field.text!) {
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
            HKObjectType.quantityTypeForIdentifier(HKQuantityTypeIdentifierOxygenSaturation)!
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
        let typeOfSpO2 = HKSampleType.quantityTypeForIdentifier(HKQuantityTypeIdentifierOxygenSaturation)
        let calendar: NSCalendar! = NSCalendar.init(identifier: NSCalendarIdentifierGregorian)
        let now: NSDate = NSDate()
        let startDate = calendar.startOfDayForDate(now)
        let endDate = calendar.dateByAddingUnit(NSCalendarUnit.Day, value: 1, toDate: startDate, options: NSCalendarOptions.MatchFirst)
        let predicate = HKQuery.predicateForSamplesWithStartDate(startDate, endDate: endDate, options: HKQueryOptions.None)
        
        // データ取得時に登録された時間でソートするためのDescriptorを生成.
        let mySortDescriptor = NSSortDescriptor(key:HKSampleSortIdentifierStartDate, ascending: false)
        
        // 体重データ読み出しのためのqueryを生成.
        let mySampleQuery = HKSampleQuery(sampleType: typeOfSpO2!, predicate: predicate, limit: 1, sortDescriptors: [mySortDescriptor])
        { (sampleQuery, results, error ) -> Void in
            // 一番最近に登録されたデータを取得.
            guard let myRecentSample = results!.first as? HKQuantitySample else {
                print("error")
                self.myReadSpO2Field.text = "Data is not found"
                return
            }
            // 取得したサンプルを単位に合わせる.
            dispatch_async(dispatch_get_main_queue(),{
                self.myReadSpO2Field.text = "\(myRecentSample.quantity)"
            })
        }
        
        // queryを発行.
        self.myHealthStore.executeQuery(mySampleQuery)
        
    }
    /*
     データの書き込み.
     */
    private func writeData(SpO2: Double){
        // 登録用データタイプを生成.
        let typeOfSpO2 = HKObjectType.quantityTypeForIdentifier(HKQuantityTypeIdentifierOxygenSaturation)!
        // 単位とセットのデータを生成.
        let percent = HKUnit.percentUnit()
        let mySpO2 = HKQuantity(unit:percent, doubleValue: SpO2)
        // StoreKit保存用データを作成.
        let mySpO2Data = HKQuantitySample(type:typeOfSpO2, quantity:mySpO2, startDate: NSDate(), endDate: NSDate())
        // HealthStoreにデータを保存.
        myHealthStore.saveObject(mySpO2Data, withCompletion: {success, error in
            if let e = error {
                print("Error: \(e.localizedDescription)")
                return
            }
            print(success ? "Success" : "Failure")
        })
    }
}
```

## Results Identifiers

|Results Identifiers(検査結果)|意味|単位(HKUnit)|
|:-:|:-:|:-:|
|HKQuantityTypeIdentifierOxygenSaturation|酸素飽和度|percent|
|HKQuantityTypeIdentifierPeripheralPerfusionIndex|末梢灌流指数|percent|
|HKQuantityTypeIdentifierBloodGlucose|血糖値|mass/volume|
|HKQuantityTypeIdentifierNumberOfTimesFallen|転倒回数|count|
|HKQuantityTypeIdentifierElectrodermalActivity|電気活動|conductance|
|HKQuantityTypeIdentifierInhalerUsage|吸入器の使用回数|count|
|HKQuantityTypeIdentifierBloodAlcoholContent|血中アルコール濃度|percent|
|HKQuantityTypeIdentifierForcedVitalCapacity|肺活量|volume|
|HKQuantityTypeIdentifierForcedExpiratoryVolume1|呼吸量|volume|
|HKQuantityTypeIdentifierPeakExpiratoryFlowRate|ピークフロー値|volume/time|

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
