# Basic ECG(IEEE Std 11073-10406 Dev specialization)

![Preview healthkit005](./img/healthkit005.png) 

## Swift3.0

`Info.plist`に`NSHealthUpdateUsageDescription`と`NSHealthShareUsageDescription`を追加します

※ Capabilities -> HealthKit をONにしてください。

![Preview healthkit001_3](./img/healthkit001_3.png)

```swift
//
//  ViewController.swift
//  healthkit005
//
//  Copyright © 2016年 FaBo, Inc. All rights reserved.
//
import UIKit
import HealthKit

class ViewController: UIViewController, UITextFieldDelegate{
    // 各インスタンスの生成.
    var myHealthStore = HKHealthStore()
    var myReadHeartRateField: UITextField!
    var myWriteHeartRateField: UITextField!
    var myReadButton: UIButton!
    var myWriteButton: UIButton!
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        // 入力フィールドを設置.
        myWriteHeartRateField = UITextField(frame: CGRect(x: 0, y: 0, width: 300, height: 30))
        myWriteHeartRateField.placeholder = "心拍数を入力してください"
        myWriteHeartRateField.delegate = self
        myWriteHeartRateField.borderStyle = UITextBorderStyle.roundedRect
        myWriteHeartRateField.layer.position = CGPoint(x: self.view.bounds.width/2, y: 200);
        self.view.addSubview(myWriteHeartRateField)
        
        // 書き込みボタンを設置.
        myWriteButton = UIButton()
        myWriteButton.frame = CGRect(x: 0, y: 0, width: 300, height: 40)
        myWriteButton.backgroundColor = UIColor.blue
        myWriteButton.layer.masksToBounds = true
        myWriteButton.setTitle("心拍数の書き込み", for: UIControlState.normal)
        myWriteButton.setTitleColor(UIColor.white, for: UIControlState.normal)
        myWriteButton.setTitleColor(UIColor.black, for: UIControlState.highlighted)
        myWriteButton.layer.cornerRadius = 20.0
        myWriteButton.layer.position = CGPoint(x: self.view.frame.width/2, y:250)
        myWriteButton.tag = 2
        myWriteButton.addTarget(self, action: #selector(ViewController.onClickMyButton(sender:)), for: .touchUpInside)
        self.view.addSubview(myWriteButton)
        
        // 表示フィールドを設置.
        myReadHeartRateField = UITextField(frame: CGRect(x: 0, y: 0, width: 300, height: 30))
        myReadHeartRateField.placeholder = "前回登録の心拍数"
        myReadHeartRateField.isEnabled = false
        myReadHeartRateField.delegate = self
        myReadHeartRateField.borderStyle = UITextBorderStyle.roundedRect
        myReadHeartRateField.layer.position = CGPoint(x:self.view.bounds.width/2,y:350);
        myReadHeartRateField.isEnabled = false
        self.view.addSubview(myReadHeartRateField)
        
        // 読み込みボタンを設置.
        myReadButton = UIButton()
        myReadButton.frame = CGRect(x: 0, y: 0, width: 300, height: 40)
        myReadButton.backgroundColor = UIColor.red
        myReadButton.layer.masksToBounds = true
        myReadButton.setTitle("心拍数の読み込み", for: UIControlState.normal)
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
            if let val = Double(myWriteHeartRateField.text!) {
                writeData(HeartRate: val)
            }
        }
    }
    
    /*
     Healthデータへのアクセスを申請.
     */
    private func requestAuthorization(){
        // 読み込みを許可する型.
        let types = Set(arrayLiteral:
            HKObjectType.quantityType(forIdentifier: HKQuantityTypeIdentifier.heartRate)!
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
        let typeOfHeartRate = HKSampleType.quantityType(forIdentifier: HKQuantityTypeIdentifier.heartRate)
        let calendar = Calendar.init(identifier: Calendar.Identifier.gregorian)
        let now = Date()
        let startDate = calendar.startOfDay(for: now)
        let endDate = calendar.date(byAdding: Calendar.Component.day, value: 1, to: startDate)
        let predicate = HKQuery.predicateForSamples(withStart: startDate, end: endDate, options: [])
        
        // データ取得時に登録された時間でソートするためのDescriptorを生成.
        let mySortDescriptor = NSSortDescriptor(key:HKSampleSortIdentifierStartDate, ascending: false)
        // queryを生成.
        let mySampleQuery = HKSampleQuery(sampleType: typeOfHeartRate!, predicate: predicate, limit: 1, sortDescriptors: [mySortDescriptor])
        { (sampleQuery, results, error ) -> Void in
            // 一番最近に登録されたデータを取得.
            guard let myRecentSample = results!.first as? HKQuantitySample else {
                print("error")
                self.myReadHeartRateField.text = "Data is not found"
                return
            }
            // 取得したサンプルを単位に合わせる.
            DispatchQueue.main.async {
                self.myReadHeartRateField.text = "\(myRecentSample.quantity)"
            }
        }
        // queryを発行.
        self.myHealthStore.execute(mySampleQuery)
    }
    
    /*
     データの書き込み.
     */
    private func writeData(HeartRate: Double){
        // 登録用データタイプを生成.
        let typeOfHeartRate = HKObjectType.quantityType(forIdentifier: HKQuantityTypeIdentifier.heartRate)
        // 単位とセットのデータを生成.
        let count = HKUnit.count()
        let minute = HKUnit.minute()
        let countParMinute = count.unitMultiplied(by: minute.reciprocal())
        let myHeartRate = HKQuantity(unit:countParMinute, doubleValue: HeartRate)
        // StoreKit保存用データを作成.
        let myHeartRateData = HKQuantitySample(type: typeOfHeartRate!, quantity: myHeartRate, start: Date(), end: Date())
        // HealthStoreにデータを保存.
        myHealthStore.save(myHeartRateData, withCompletion: {success, error in
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
//  healthkit005
//
//  Copyright © 2016年 FaBo, Inc. All rights reserved.
//
import UIKit
import HealthKit

class ViewController: UIViewController, UITextFieldDelegate {
    // 各インスタンスの生成.
    var myHealthStore = HKHealthStore()
    var myReadHeartRateField: UITextField!
    var myWriteHeartRateField: UITextField!
    var myReadButton: UIButton!
    var myWriteButton: UIButton!
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        // 入力フィールドを設置.
        myWriteHeartRateField = UITextField(frame: CGRectMake(0,0,300,30))
        myWriteHeartRateField.placeholder = "心拍数を入力してください"
        myWriteHeartRateField.delegate = self
        myWriteHeartRateField.borderStyle = UITextBorderStyle.RoundedRect
        myWriteHeartRateField.layer.position = CGPoint(x:self.view.bounds.width/2,y:200);
        self.view.addSubview(myWriteHeartRateField)
        
        // 書き込みボタンを設置.
        myWriteButton = UIButton()
        myWriteButton.frame = CGRectMake(0,0,300,40)
        myWriteButton.backgroundColor = UIColor.blueColor();
        myWriteButton.layer.masksToBounds = true
        myWriteButton.setTitle("心拍数の書き込み", forState: UIControlState.Normal)
        myWriteButton.setTitleColor(UIColor.whiteColor(), forState: UIControlState.Normal)
        myWriteButton.setTitleColor(UIColor.blackColor(), forState: UIControlState.Highlighted)
        myWriteButton.layer.cornerRadius = 20.0
        myWriteButton.layer.position = CGPoint(x: self.view.frame.width/2, y:250)
        myWriteButton.tag = 2
        myWriteButton.addTarget(self, action: #selector(ViewController.onClickMyButton(_:)), forControlEvents: .TouchUpInside)
        self.view.addSubview(myWriteButton)
        
        
        // 表示フィールドを設置.
        myReadHeartRateField = UITextField(frame: CGRectMake(0,0,300,30))
        myReadHeartRateField.placeholder = "前回登録の心拍数"
        myReadHeartRateField.enabled = false
        myReadHeartRateField.delegate = self
        myReadHeartRateField.borderStyle = UITextBorderStyle.RoundedRect
        myReadHeartRateField.layer.position = CGPoint(x:self.view.bounds.width/2,y:350);
        myReadHeartRateField.enabled = false
        self.view.addSubview(myReadHeartRateField)
        
        // 読み込みボタンを設置.
        myReadButton = UIButton()
        myReadButton.frame = CGRectMake(0,0,300,40)
        myReadButton.backgroundColor = UIColor.redColor();
        myReadButton.layer.masksToBounds = true
        myReadButton.setTitle("心拍数の読み込み", forState: UIControlState.Normal)
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
            if let val = Double(myWriteHeartRateField.text!) {
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
            HKObjectType.quantityTypeForIdentifier(HKQuantityTypeIdentifierHeartRate)!
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
        let typeOfHeartRate = HKSampleType.quantityTypeForIdentifier(HKQuantityTypeIdentifierHeartRate)
        let calendar = NSCalendar.init(identifier: NSCalendarIdentifierGregorian)
        let now = NSDate()
        let startDate = calendar!.startOfDayForDate(now)
        let endDate = calendar!.dateByAddingUnit(NSCalendarUnit.Day, value: 1, toDate: startDate, options: NSCalendarOptions.MatchFirst)
        let predicate = HKQuery.predicateForSamplesWithStartDate(startDate, endDate: endDate, options: HKQueryOptions.None)
        
        // データ取得時に登録された時間でソートするためのDescriptorを生成.
        let mySortDescriptor = NSSortDescriptor(key:HKSampleSortIdentifierStartDate, ascending: false)
        
        // queryを生成.
        let mySampleQuery = HKSampleQuery(sampleType: typeOfHeartRate!, predicate: predicate, limit: 1, sortDescriptors: [mySortDescriptor])
        { (sampleQuery, results, error ) -> Void in
            // 一番最近に登録されたデータを取得.
            guard let myRecentSample = results!.first as? HKQuantitySample else {
                print("error")
                self.myReadHeartRateField.text = "Data is not found"
                return
            }
            // 取得したサンプルを単位に合わせる.
            dispatch_async(dispatch_get_main_queue(),{
                self.myReadHeartRateField.text = "\(myRecentSample.quantity)"
            })
        }
        
        // queryを発行.
        self.myHealthStore.executeQuery(mySampleQuery)
    }
    
    /*
     データの書き込み.
     */
    private func writeData(HeartRate: Double){
        // 登録用データタイプを生成.
        let typeOfHeartRate = HKObjectType.quantityTypeForIdentifier(HKQuantityTypeIdentifierHeartRate)
        // 単位とセットのデータを生成.
        let count = HKUnit.countUnit()
        let minute = HKUnit.minuteUnit()
        let countParMinute = count.unitMultipliedByUnit(minute.reciprocalUnit())
        let myHeartRate = HKQuantity(unit:countParMinute, doubleValue: HeartRate)
        // StoreKit保存用データを作成.
        let myHeartRateData = HKQuantitySample(type: typeOfHeartRate!, quantity: myHeartRate, startDate: NSDate(), endDate: NSDate())
        // HealthStoreにデータを保存.
        myHealthStore.saveObject(myHeartRateData, withCompletion: {success, error in
            if let e = error {
                print("Error: \(e.localizedDescription)")
                return
            }
            print(success ? "Success" : "Failure")
        })
    }
}
```

## Vital Signs Identifiers

|Vital Signs Identifiers (健康状況)|意味|単位(HKUnit)|
|HKQuantityTypeIdentifierHeartRate|心拍数|count/time|
|HKQuantityTypeIdentifierBodyTemperature|体温|temperature|
|HKQuantityTypeIdentifierBloodPressureSystolic|収縮期血圧(最高血圧)|pressure|
|HKQuantityTypeIdentifierBloodPressureDiastolic|拡張期血圧(最低血圧)|pressure|
|HKQuantityTypeIdentifierRespiratoryRate|呼吸数|count/time|

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
