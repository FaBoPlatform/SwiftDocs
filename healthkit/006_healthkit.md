# Blood pressure monitor(IEEE Std 11073-10407 Dev specialization)

![Preview healthkit006](./img/healthkit006.png)

## Swift3.0

`Info.plist`に`NSHealthUpdateUsageDescription`と`NSHealthShareUsageDescription`を追加します

※ Capabilities -> HealthKit をONにしてください。

![Preview healthkit001_3](./img/healthkit001_3.png)

```swift
//
//  ViewController.swift
//  healthkit006
//
//  Copyright © 2016年 FaBo, Inc. All rights reserved.
//
import UIKit
import HealthKit

class ViewController: UIViewController, UITextFieldDelegate {
    // 各インスタンスの生成.
    var myHealthStore = HKHealthStore()
    var myWriteBPSystolicField: UITextField!
    var myWriteBPDiastolicField:UITextField!
    var myReadBPSystolicField: UITextField!
    var myReadBPDiastolicField:UITextField!
    var myReadButton: UIButton!
    var myWriteButton: UIButton!
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        // 入力フィールドを設置.
        myWriteBPSystolicField = UITextField(frame: CGRect(x: 0, y: 0, width: 300, height: 30))
        myWriteBPSystolicField.placeholder = "収縮期血圧(最高血圧)を入力してください"
        myWriteBPSystolicField.delegate = self
        myWriteBPSystolicField.borderStyle = UITextBorderStyle.roundedRect
        myWriteBPSystolicField.layer.position = CGPoint(x:self.view.bounds.width/2,y:150);
        self.view.addSubview(myWriteBPSystolicField)
        
        myWriteBPDiastolicField = UITextField(frame: CGRect(x: 0, y: 0, width: 300, height: 30))
        myWriteBPDiastolicField.placeholder = "拡張期血圧(最低血圧)を入力してください"
        myWriteBPDiastolicField.delegate = self
        myWriteBPDiastolicField.borderStyle = UITextBorderStyle.roundedRect
        myWriteBPDiastolicField.layer.position = CGPoint(x:self.view.bounds.width/2,y:200);
        self.view.addSubview(myWriteBPDiastolicField)
        
        // 書き込みボタンを設置.
        myWriteButton = UIButton()
        myWriteButton.frame = CGRect(x: 0, y: 0, width: 300, height: 30)
        myWriteButton.backgroundColor = UIColor.blue
        myWriteButton.layer.masksToBounds = true
        myWriteButton.setTitle("血圧の書き込み", for: UIControlState.normal)
        myWriteButton.setTitleColor(UIColor.white, for: UIControlState.normal)
        myWriteButton.setTitleColor(UIColor.black, for: UIControlState.highlighted)
        myWriteButton.layer.cornerRadius = 20.0
        myWriteButton.layer.position = CGPoint(x: self.view.frame.width/2, y:250)
        myWriteButton.tag = 2
        myWriteButton.addTarget(self, action: #selector(ViewController.onClickMyButton(sender:)
            ), for: .touchUpInside)
        self.view.addSubview(myWriteButton)
        
        
        // 表示フィールドを設置.
        myReadBPSystolicField = UITextField(frame: CGRect(x: 0, y: 0, width: 300, height: 30))
        myReadBPSystolicField.placeholder = "前回登録の収縮期血圧(最高血圧)"
        myReadBPSystolicField.isEnabled = false
        myReadBPSystolicField.delegate = self
        myReadBPSystolicField.borderStyle = UITextBorderStyle.roundedRect
        myReadBPSystolicField.layer.position = CGPoint(x:self.view.bounds.width/2,y:350);
        myReadBPSystolicField.isEnabled = false
        self.view.addSubview(myReadBPSystolicField)
        
        myReadBPDiastolicField = UITextField(frame: CGRect(x: 0, y: 0, width: 300, height: 30))
        myReadBPDiastolicField.placeholder = "前回登録の拡張期血圧(最低血圧)"
        myReadBPDiastolicField.isEnabled = false
        myReadBPDiastolicField.delegate = self
        myReadBPDiastolicField.borderStyle = UITextBorderStyle.roundedRect
        myReadBPDiastolicField.layer.position = CGPoint(x:self.view.bounds.width/2,y:400);
        myReadBPDiastolicField.isEnabled = false
        self.view.addSubview(myReadBPDiastolicField)
        
        // 読み込みボタンを設置.
        myReadButton = UIButton()
        myReadButton.frame = CGRect(x: 0, y: 0, width: 300, height: 30)
        myReadButton.backgroundColor = UIColor.red
        myReadButton.layer.masksToBounds = true
        myReadButton.setTitle("血圧の読み込み", for: UIControlState.normal)
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
            let bps = Double(myWriteBPSystolicField.text!)
            let bpd = Double(myWriteBPDiastolicField.text!)
            if bps != nil && bpd != nil {
                writeData(BPSystolic: bps!, BPDiastolic: bpd!)
            }
        }
    }
    
    /*
     Healthデータへのアクセスを申請.
     */
    private func requestAuthorization(){
        // 読み込みを許可する型.
        let types = Set(arrayLiteral:
            HKObjectType.quantityType(forIdentifier: HKQuantityTypeIdentifier.bloodPressureSystolic)!,
                        HKObjectType.quantityType(forIdentifier: HKQuantityTypeIdentifier.bloodPressureDiastolic)!
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
        let typeOfBPSystolic = HKQuantityType.quantityType(forIdentifier: HKQuantityTypeIdentifier.bloodPressureSystolic)
        let typeOfBPDiastolic = HKQuantityType.quantityType(forIdentifier: HKQuantityTypeIdentifier.bloodPressureDiastolic)
        
        let calendar = Calendar.init(identifier: Calendar.Identifier.gregorian)
        let now = Date()
        let startDate = calendar.startOfDay(for: now)
        let endDate = calendar.date(byAdding: Calendar.Component.day, value: 1, to: startDate)
        let predicate = HKQuery.predicateForSamples(withStart: startDate, end: endDate, options: [])
        
        // データ取得時に登録された時間でソートするためのDescriptorを生成.
        let mySortDescriptor = NSSortDescriptor(key:HKSampleSortIdentifierStartDate, ascending: false)
        
        // queryを生成.
        let mySampleQueryBPS = HKSampleQuery(sampleType: typeOfBPSystolic!, predicate: predicate, limit: 1, sortDescriptors: [mySortDescriptor])
        { (sampleQuery, results, error ) -> Void in
            // 一番最近に登録されたデータを取得.
            guard let myRecentSample = results!.first as? HKQuantitySample else {
                print("error")
                self.myReadBPSystolicField.text = "Data is not found"
                return
            }
            // 取得したサンプルを単位に合わせる.
            DispatchQueue.main.async {
                self.myReadBPSystolicField.text = "\(myRecentSample.quantity)"
            }
        }
        let mySampleQueryBPD = HKSampleQuery(sampleType: typeOfBPDiastolic!, predicate: predicate, limit: 1, sortDescriptors: [mySortDescriptor])
        { (sampleQuery, results, error ) -> Void in
            guard let myRecentSample = results!.first as? HKQuantitySample else {
                print("error")
                self.myReadBPDiastolicField.text = "Data is not found"
                return
            }
            DispatchQueue.main.async {
                self.myReadBPDiastolicField.text = "\(myRecentSample.quantity)"
            }
        }
        
        // queryを発行.
        self.myHealthStore.execute(mySampleQueryBPS)
        self.myHealthStore.execute(mySampleQueryBPD)
    }
    
    /*
     データの書き込み.
     */
    private func writeData(BPSystolic: Double, BPDiastolic: Double){
        
        // BodyMassの登録用データタイプ.
        let typeOfBPSystolic = HKQuantityType.quantityType(forIdentifier: HKQuantityTypeIdentifier.bloodPressureSystolic)
        let typeOfBPDiastolic = HKQuantityType.quantityType(forIdentifier: HKQuantityTypeIdentifier.bloodPressureDiastolic)
        
        // 単位とセットのデータを生成.
        let pa = HKUnit.pascal()
        let myBPSystolic = HKQuantity(unit:pa, doubleValue: BPSystolic)
        let myBPDiastolic = HKQuantity(unit:pa, doubleValue: BPDiastolic)
        
        // StoreKit保存用データを作成.
        let myBPSystolicData = HKQuantitySample(type: typeOfBPSystolic!, quantity: myBPSystolic, start: Date(), end: Date())
        let myBPDiastolicData = HKQuantitySample(type: typeOfBPDiastolic!, quantity: myBPDiastolic, start: Date(), end: Date())
        
        // 保存データを一つの配列にまとめる.
        //let mySaveObjects = NSArray(array: [myBPSystolicData,myBPDiastolicData])
        
        // HealthStoreに登録.
        myHealthStore.save([myBPSystolicData, myBPDiastolicData], withCompletion: { success, error in
            print(success ? "Success" : "Failure")
        })
    }
}
```

## Swift2.3

```swift
//
//  ViewController.swift
//  healthkit006
//
//  Copyright © 2016年 FaBo, Inc. All rights reserved.
//
import UIKit
import HealthKit

class ViewController: UIViewController, UITextFieldDelegate {
    // 各インスタンスの生成.
    var myHealthStore = HKHealthStore()
    var myWriteBPSystolicField: UITextField!
    var myWriteBPDiastolicField:UITextField!
    var myReadBPSystolicField: UITextField!
    var myReadBPDiastolicField:UITextField!
    var myReadButton: UIButton!
    var myWriteButton: UIButton!
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        // 入力フィールドを設置.
        myWriteBPSystolicField = UITextField(frame: CGRectMake(0,0,300,30))
        myWriteBPSystolicField.placeholder = "収縮期血圧(最高血圧)を入力してください"
        myWriteBPSystolicField.delegate = self
        myWriteBPSystolicField.borderStyle = UITextBorderStyle.RoundedRect
        myWriteBPSystolicField.layer.position = CGPoint(x:self.view.bounds.width/2,y:150);
        self.view.addSubview(myWriteBPSystolicField)
        
        myWriteBPDiastolicField = UITextField(frame: CGRectMake(0,0,300,30))
        myWriteBPDiastolicField.placeholder = "拡張期血圧(最低血圧)を入力してください"
        myWriteBPDiastolicField.delegate = self
        myWriteBPDiastolicField.borderStyle = UITextBorderStyle.RoundedRect
        myWriteBPDiastolicField.layer.position = CGPoint(x:self.view.bounds.width/2,y:200);
        self.view.addSubview(myWriteBPDiastolicField)
        
        // 書き込みボタンを設置.
        myWriteButton = UIButton()
        myWriteButton.frame = CGRectMake(0,0,300,40)
        myWriteButton.backgroundColor = UIColor.blueColor();
        myWriteButton.layer.masksToBounds = true
        myWriteButton.setTitle("血圧の書き込み", forState: UIControlState.Normal)
        myWriteButton.setTitleColor(UIColor.whiteColor(), forState: UIControlState.Normal)
        myWriteButton.setTitleColor(UIColor.blackColor(), forState: UIControlState.Highlighted)
        myWriteButton.layer.cornerRadius = 20.0
        myWriteButton.layer.position = CGPoint(x: self.view.frame.width/2, y:250)
        myWriteButton.tag = 2
        myWriteButton.addTarget(self, action: #selector(ViewController.onClickMyButton(_:)), forControlEvents: .TouchUpInside)
        self.view.addSubview(myWriteButton)
        
        
        // 表示フィールドを設置.
        myReadBPSystolicField = UITextField(frame: CGRectMake(0,0,300,30))
        myReadBPSystolicField.placeholder = "前回登録の収縮期血圧(最高血圧)"
        myReadBPSystolicField.enabled = false
        myReadBPSystolicField.delegate = self
        myReadBPSystolicField.borderStyle = UITextBorderStyle.RoundedRect
        myReadBPSystolicField.layer.position = CGPoint(x:self.view.bounds.width/2,y:350);
        myReadBPSystolicField.enabled = false
        self.view.addSubview(myReadBPSystolicField)
        
        myReadBPDiastolicField = UITextField(frame: CGRectMake(0,0,300,30))
        myReadBPDiastolicField.placeholder = "前回登録の拡張期血圧(最低血圧)"
        myReadBPDiastolicField.enabled = false
        myReadBPDiastolicField.delegate = self
        myReadBPDiastolicField.borderStyle = UITextBorderStyle.RoundedRect
        myReadBPDiastolicField.layer.position = CGPoint(x:self.view.bounds.width/2,y:400);
        myReadBPDiastolicField.enabled = false
        self.view.addSubview(myReadBPDiastolicField)
        
        // 読み込みボタンを設置.
        myReadButton = UIButton()
        myReadButton.frame = CGRectMake(0,0,300,40)
        myReadButton.backgroundColor = UIColor.redColor();
        myReadButton.layer.masksToBounds = true
        myReadButton.setTitle("血圧の読み込み", forState: UIControlState.Normal)
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
            let bps = Double(myWriteBPSystolicField.text!)
            let bpd = Double(myWriteBPDiastolicField.text!)
            if bps != nil && bpd != nil {
                writeData(bps!, BPDiastolic: bpd!)
            }
        }
    }
    
    /*
     Healthデータへのアクセスを申請.
     */
    private func requestAuthorization(){
        // 読み込みを許可する型.
        let types = Set(arrayLiteral:
            HKObjectType.quantityTypeForIdentifier(HKQuantityTypeIdentifierBloodPressureSystolic)!,
                        HKObjectType.quantityTypeForIdentifier(HKQuantityTypeIdentifierBloodPressureDiastolic)!
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
        let typeOfBPSystolic = HKQuantityType.quantityTypeForIdentifier(HKQuantityTypeIdentifierBloodPressureSystolic)
        let typeOfBPDiastolic = HKQuantityType.quantityTypeForIdentifier(HKQuantityTypeIdentifierBloodPressureDiastolic)
        
        let calendar = NSCalendar.init(identifier: NSCalendarIdentifierGregorian)
        let now = NSDate()
        let startDate = calendar!.startOfDayForDate(now)
        let endDate = calendar!.dateByAddingUnit(NSCalendarUnit.Day, value: 1, toDate: startDate, options: NSCalendarOptions.MatchFirst)
        let predicate = HKQuery.predicateForSamplesWithStartDate(startDate, endDate: endDate, options: HKQueryOptions.None)
        
        // データ取得時に登録された時間でソートするためのDescriptorを生成.
        let mySortDescriptor = NSSortDescriptor(key:HKSampleSortIdentifierStartDate, ascending: false)
        
        // queryを生成.
        let mySampleQueryBPS = HKSampleQuery(sampleType: typeOfBPSystolic!, predicate: predicate, limit: 1, sortDescriptors: [mySortDescriptor])
        { (sampleQuery, results, error ) -> Void in
            // 一番最近に登録されたデータを取得.
            guard let myRecentSample = results!.first as? HKQuantitySample else {
                print("error")
                self.myReadBPSystolicField.text = "Data is not found"
                return
            }
            // 取得したサンプルを単位に合わせる.
            dispatch_async(dispatch_get_main_queue(),{
                self.myReadBPSystolicField.text = "\(myRecentSample.quantity)"
            })
        }
        let mySampleQueryBPD = HKSampleQuery(sampleType: typeOfBPDiastolic!, predicate: predicate, limit: 1, sortDescriptors: [mySortDescriptor])
        { (sampleQuery, results, error ) -> Void in
            // 一番最近に登録されたデータを取得.
            guard let myRecentSample = results!.first as? HKQuantitySample else {
                print("error")
                self.myReadBPDiastolicField.text = "Data is not found"
                return
            }
            // 取得したサンプルを単位に合わせる.
            dispatch_async(dispatch_get_main_queue(),{
                self.myReadBPDiastolicField.text = "\(myRecentSample.quantity)"
            })
        }
        
        // queryを発行.
        self.myHealthStore.executeQuery(mySampleQueryBPS)
        self.myHealthStore.executeQuery(mySampleQueryBPD)
    }
    
    /*
     データの書き込み.
     */
    private func writeData(BPSystolic: Double, BPDiastolic: Double){
        
        // BodyMassの登録用データタイプ.
        let typeOfBPSystolic = HKQuantityType.quantityTypeForIdentifier(HKQuantityTypeIdentifierBloodPressureSystolic)
        let typeOfBPDiastolic = HKQuantityType.quantityTypeForIdentifier(HKQuantityTypeIdentifierBloodPressureDiastolic)
        
        // 単位とセットのデータを生成.
        let pa = HKUnit.pascalUnit()
        let myBPSystolic = HKQuantity(unit:pa, doubleValue: BPSystolic)
        let myBPDiastolic = HKQuantity(unit:pa, doubleValue: BPDiastolic)
        
        // StoreKit保存用データを作成.
        let myBPSystolicData = HKQuantitySample(type:typeOfBPSystolic!, quantity:myBPSystolic, startDate:NSDate(), endDate:NSDate())
        let myBPDiastolicData = HKQuantitySample(type:typeOfBPDiastolic!, quantity:myBPDiastolic, startDate:NSDate(), endDate:NSDate())
        
        // 保存データを一つの配列にまとめる.
        //let mySaveObjects = NSArray(array: [myBPSystolicData,myBPDiastolicData])
        
        // HealthStoreに登録.
        myHealthStore.saveObjects([myBPSystolicData, myBPDiastolicData], withCompletion: { success, error in
            print(success ? "Success" : "Failure")
        })
    }
}
```

## Vital Signs Identifiers

|Vital Signs Identifiers (健康状況)|意味|単位(HKUnit)|
|:-:|:-:|:-:|
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
