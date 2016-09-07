# Body comp (IEEE Std 11073-10420 Dev specialization)

![Preview healthkit001](./img/healthkit002.png) ![Preview healthkit001_2](./img/healthkit002_2.png)

## Swift3.0

`Info.plist`に`NSHealthUpdateUsageDescription`と`NSHealthShareUsageDescription`を追加します

※ Capabilities -> HealthKit をONにしてください。

![Preview healthkit001_3](./img/healthkit001_3.png)

```swift
//
//  ViewController.swift
//  healthkit010
//
//  Copyright © 2016年 FaBo, Inc. All rights reserved.
//
import UIKit
import HealthKit

class ViewController:UIViewController, UITextFieldDelegate {
    
    // 各インスタンスの生成.
    var myHealthStore:HKHealthStore = HKHealthStore()
    
    var myReadBodyMassField: UITextField = UITextField()
    var myReadHeightField:UITextField = UITextField()
    var myReadBodyFatField:UITextField = UITextField()
    var myReadLeanMassField:UITextField = UITextField()
    var myReadBMIField:UITextField = UITextField()
    
    var myWriteBodyMassField: UITextField = UITextField()
    var myWriteHeightField:UITextField = UITextField()
    var myWriteBMIField:UITextField = UITextField()
    var myWriteBodyFatField:UITextField = UITextField()
    var myWriteLeanMassField:UITextField = UITextField()
    
    var myReadButton: UIButton!
    var myWriteButton: UIButton!
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        // HealthStoreの生成.
        myHealthStore = HKHealthStore()
        
        // 体重用の入力フィールドを設置.
        self.setTextField(textField: myWriteBodyMassField, placeholder: "体重を入力してください", position: CGPoint(x:self.view.bounds.width/2,y:85))
        // 身長用の入力フィールドを設置.
        self.setTextField(textField: myWriteHeightField, placeholder: "身長を入力してください", position: CGPoint(x:self.view.bounds.width/2,y:120))
        // BMI用の入力フィールドを設置(自動計算).
        myWriteBMIField.isEnabled = false
        self.setTextField(textField: myWriteBMIField, placeholder: "BMIの計算値", position: CGPoint(x:self.view.bounds.width/2,y:155))
        // 体脂肪率用の入力フィールドを設置(自動計算).
        self.setTextField(textField: myWriteBodyFatField, placeholder: "体脂肪率を入力してください", position: CGPoint(x:self.view.bounds.width/2,y:190))
        // 除脂肪体重用の入力フィールドを設置(自動計算).
        self.setTextField(textField: myWriteLeanMassField, placeholder: "除脂肪体重を入力してください", position: CGPoint(x:self.view.bounds.width/2,y:225))
        
        // 書き込みボタンを設置.
        myWriteButton = UIButton()
        myWriteButton.frame = CGRect(x: 0, y: 0, width: 300, height: 40)
        myWriteButton.backgroundColor = UIColor.blue
        myWriteButton.layer.masksToBounds = true
        myWriteButton.setTitle("体重の書き込み", for: UIControlState.normal)
        myWriteButton.setTitleColor(UIColor.white, for: UIControlState.normal)
        myWriteButton.setTitleColor(UIColor.black, for: UIControlState.highlighted)
        myWriteButton.layer.cornerRadius = 20.0
        myWriteButton.layer.position = CGPoint(x: self.view.frame.width/2, y:280)
        myWriteButton.tag = 2
        myWriteButton.addTarget(self, action: #selector(ViewController.onClickMyButton(sender:)), for: .touchUpInside)
        self.view.addSubview(myWriteButton);
        
        // 体重用の表示フィールドを設置.
        myReadBodyMassField.isEnabled = false
        self.setTextField(textField: myReadBodyMassField, placeholder: "前回登録の体重", position: CGPoint(x:self.view.bounds.width/2,y:350))
        // 身長用の表示フィールドを設置.
        myReadHeightField.isEnabled = false
        self.setTextField(textField: myReadHeightField, placeholder: "前回登録の身長", position: CGPoint(x:self.view.bounds.width/2,y:385))
        // BMI用の表示フィールドを設置.
        myReadBMIField.isEnabled = false
        self.setTextField(textField: myReadBMIField, placeholder: "前回登録のBMI", position: CGPoint(x:self.view.bounds.width/2,y:420))
        // 体脂肪率用の表示フィールドを設置.
        myReadBodyFatField.isEnabled = false
        self.setTextField(textField: myReadBodyFatField, placeholder: "前回登録の体脂肪率", position: CGPoint(x:self.view.bounds.width/2,y:490))
        // 除脂肪体重用の表示フィールドを設置.
        myReadLeanMassField.isEnabled = false
        self.setTextField(textField: myReadLeanMassField, placeholder: "前回登録の除脂肪体重", position: CGPoint(x:self.view.bounds.width/2,y:455))
        
        // 読み込みボタンを設置.
        myReadButton = UIButton()
        myReadButton.frame = CGRect(x: 0, y: 0, width: 300, height: 40)
        myReadButton.backgroundColor = UIColor.red
        myReadButton.layer.masksToBounds = true
        myReadButton.setTitle("体重の読み込み", for: UIControlState.normal)
        myReadButton.setTitleColor(UIColor.white, for: UIControlState.normal)
        myReadButton.setTitleColor(UIColor.black, for: UIControlState.highlighted)
        myReadButton.layer.cornerRadius = 20.0
        myReadButton.layer.position = CGPoint(x: self.view.frame.width/2, y:540)
        myReadButton.tag = 1
        myReadButton.addTarget(self, action: #selector(ViewController.onClickMyButton(sender:)), for: .touchUpInside)
        self.view.addSubview(myReadButton);
    }
    
    override func viewDidAppear(_ animated: Bool) {
        super.viewDidAppear(animated)
        // HealthStoreへの許可を申請.
        requestAuthorization()
    }
    
    /*
     TextFieldの初期化用関数.
     */
    private func setTextField(textField:UITextField,placeholder:String,position:CGPoint){
        
        textField.frame = CGRect(x: 0, y: 0, width: 300, height: 30)
        textField.placeholder = placeholder
        textField.delegate = self
        textField.borderStyle = UITextBorderStyle.roundedRect
        textField.layer.position = position;
        
        self.view.addSubview(textField)
    }
    
    // MARK: - UITextFieldDelegate
    
    /*
     UITextFieldが編集終了する直前に呼ばれる.
     */
    func textFieldShouldEndEditing(_ textField: UITextField) -> Bool {
        
        // 身長・体重どちらも入力済みであったらBMIの計算を行う.
        if myWriteBodyMassField.text != "" && myWriteHeightField.text != "" {
            let myBodyMass = Double(myWriteBodyMassField.text!)
            let myHeight = Double(myWriteHeightField.text!)
            if myBodyMass != nil && myHeight != nil {
                let myBMI = myBodyMass! / ((myHeight! / 100) * (myHeight! / 100))
                myWriteBMIField.text = "\(myBMI)"
            }
        }
        return true
    }
    
    /*
     Healthデータへのアクセスを申請する関数.
     */
    private func requestAuthorization(){
        // 読み込みを許可する型.
        let types = Set(arrayLiteral:
            HKObjectType.quantityType(forIdentifier: HKQuantityTypeIdentifier.bodyMass)!,
            HKObjectType.quantityType(forIdentifier: HKQuantityTypeIdentifier.height)!,
            HKObjectType.quantityType(forIdentifier: HKQuantityTypeIdentifier.bodyMassIndex)!,
            HKObjectType.quantityType(forIdentifier: HKQuantityTypeIdentifier.bodyFatPercentage)!,
            HKObjectType.quantityType(forIdentifier: HKQuantityTypeIdentifier.leanBodyMass)!
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
     ボタンイベント.
     */
    func onClickMyButton(sender: UIButton){
        if(sender.tag == 1){
            readData()
        } else if(sender.tag == 2){
            let bodyMass = Double(myWriteBodyMassField.text!)
            let height = Double(myWriteHeightField.text!)
            let bmi = Double(myWriteBMIField.text!)
            let fat = Double(myWriteBodyFatField.text!)
            let leanBodyMass = Double(myWriteLeanMassField.text!)
            
            if bodyMass != nil && height != nil && bmi != nil && fat != nil && leanBodyMass != nil {
                writeData(BodyMass: bodyMass!, Height: height!, BMI: bmi!, Fat: fat!, LeanMass: leanBodyMass!)
            }
        }
    }
    
    /*
     データの読み出し用の関数.
     */
    private func readData() {
        // 取得したいデータのタイプを生成する.
        let typeOfBodyMass:HKSampleType = HKSampleType.quantityType(forIdentifier: HKQuantityTypeIdentifier.bodyMass)!
        let typeOfHeight:HKSampleType = HKSampleType.quantityType(forIdentifier: HKQuantityTypeIdentifier.height)!
        let typeOfBMI:HKSampleType = HKSampleType.quantityType(forIdentifier: HKQuantityTypeIdentifier.bodyMassIndex)!
        let typeOfFat:HKSampleType = HKSampleType.quantityType(forIdentifier: HKQuantityTypeIdentifier.bodyFatPercentage)!
        let typeofLeanBodyMass:HKSampleType = HKSampleType.quantityType(forIdentifier: HKQuantityTypeIdentifier.leanBodyMass)!
        
        let kg = HKUnit.gramUnit(with: .kilo)
        getBodyCompData(sampletype: typeOfBodyMass, unit: kg, inputField: myReadBodyMassField)
        getBodyCompData(sampletype: typeofLeanBodyMass, unit: kg, inputField: myReadLeanMassField)
        
        let cm = HKUnit.meterUnit(with: .centi)
        getBodyCompData(sampletype: typeOfHeight, unit: cm, inputField: myReadHeightField)
        
        let count = HKUnit.count()
        getBodyCompData(sampletype: typeOfBMI, unit: count, inputField: myReadBMIField)
        
        let per = HKUnit.percent()
        getBodyCompData(sampletype: typeOfFat, unit: per, inputField: myReadBodyFatField)
    }
    
    /*
     データの読み出しのためのクエリを生成・発行するための関数.
     */
    private func getBodyCompData(sampletype:HKSampleType,unit:HKUnit,inputField:UITextField){
        let calendar = Calendar.init(identifier: Calendar.Identifier.gregorian)
        let now = Date()
        let startDate = calendar.startOfDay(for: now)
        let endDate = calendar.date(byAdding: Calendar.Component.day, value: 1, to: startDate)
        let predicate = HKQuery.predicateForSamples(withStart: startDate, end: endDate, options: [])
        // データ取得時に登録された時間でソートするためのDescriptorを生成.
        let mySortDescriptor = NSSortDescriptor(key:HKSampleSortIdentifierStartDate, ascending: false)
        // queryを生成.
        let mySampleQuery = HKSampleQuery(sampleType: sampletype, predicate: predicate, limit: 1, sortDescriptors: [mySortDescriptor])
        { (sampleQuery, results, error ) -> Void in
            // 一番最近に登録されたデータを取得.
            guard let myRecentSample = results!.first as? HKQuantitySample else {
                print("error")
                inputField.text = "Data is not found"
                return
            }
            // 取得したサンプルを単位に合わせる.
            DispatchQueue.main.async {
                inputField.text = "\(myRecentSample.quantity)"
            }
        }
        // queryを発行.
        self.myHealthStore.execute(mySampleQuery)
    }
    
    /*
     データの書き込み用の関数.
     */
    private func writeData(BodyMass: Double, Height: Double, BMI: Double, Fat: Double, LeanMass: Double){
        
        // 登録用データタイプを生成.
        let typeOfBodyMass:HKQuantityType = HKObjectType.quantityType(forIdentifier: HKQuantityTypeIdentifier.bodyMass)!
        let typeOfHeight:HKQuantityType = HKObjectType.quantityType(forIdentifier: HKQuantityTypeIdentifier.height)!
        let typeOfBMI:HKQuantityType = HKObjectType.quantityType(forIdentifier: HKQuantityTypeIdentifier.bodyMassIndex)!
        let typeOfFat:HKQuantityType = HKObjectType.quantityType(forIdentifier: HKQuantityTypeIdentifier.bodyFatPercentage)!
        let typeofLeanBodyMass:HKQuantityType = HKObjectType.quantityType(forIdentifier: HKQuantityTypeIdentifier.leanBodyMass)!
        
        // 単位とセットのデータを生成.
        let kg:HKUnit = HKUnit.gramUnit(with: .kilo)
        let myBodyMass = HKQuantity(unit:kg, doubleValue: BodyMass)
        let myLeanBodyMass = HKQuantity(unit: kg, doubleValue: LeanMass)
        
        let cm:HKUnit = HKUnit.meterUnit(with: .centi)
        let myHeight = HKQuantity(unit: cm, doubleValue: Height)
        
        let count:HKUnit = HKUnit.count()
        let myBMI = HKQuantity(unit: count , doubleValue: BMI)
        
        let per:HKUnit = HKUnit.percent()
        let myFat = HKQuantity(unit: per, doubleValue: Fat)
        
        // StoreKit保存用データを作成.
        let myBodyMassData = HKQuantitySample(type: typeOfBodyMass, quantity: myBodyMass, start: Date(), end: Date())
        let myHeightData = HKQuantitySample(type: typeOfHeight, quantity: myHeight, start: Date(), end: Date())
        let myBMIData = HKQuantitySample(type: typeOfBMI, quantity:myBMI, start: Date(), end:Date())
        let myFatData = HKQuantitySample(type: typeOfFat, quantity: myFat, start: Date(), end: Date())
        let myLeanBodyMassData = HKQuantitySample(type: typeofLeanBodyMass, quantity: myLeanBodyMass, start: Date(), end: Date())
        
        // HealthStoreにデータを保存.
        myHealthStore.save([myBodyMassData, myHeightData, myBMIData, myFatData, myLeanBodyMassData, ]) { (success, error) in
            if let e = error {
                print("Error: \(e.localizedDescription)")
                return
            }
            print(success ? "Success" : "Failure")
        }
    }
}
```

## Swift2.3

```swift
//
//  ViewController.swift
//  healthkit010
//
//  Copyright © 2016年 FaBo, Inc. All rights reserved.
//
import UIKit
import HealthKit

class ViewController:UIViewController, UITextFieldDelegate {
    
    // 各インスタンスの生成.
    var myHealthStore:HKHealthStore = HKHealthStore()
    
    var myReadBodyMassField: UITextField = UITextField()
    var myReadHeightField:UITextField = UITextField()
    var myReadBodyFatField:UITextField = UITextField()
    var myReadLeanMassField:UITextField = UITextField()
    var myReadBMIField:UITextField = UITextField()
    
    var myWriteBodyMassField: UITextField = UITextField()
    var myWriteHeightField:UITextField = UITextField()
    var myWriteBMIField:UITextField = UITextField()
    var myWriteBodyFatField:UITextField = UITextField()
    var myWriteLeanMassField:UITextField = UITextField()
    
    var myReadButton: UIButton!
    var myWriteButton: UIButton!
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        // HealthStoreの生成.
        myHealthStore = HKHealthStore()
        
        // 体重用の入力フィールドを設置.
        self.setTextField(myWriteBodyMassField, placeholder: "体重を入力してください", position: CGPoint(x:self.view.bounds.width/2,y:85))
        // 身長用の入力フィールドを設置.
        self.setTextField(myWriteHeightField, placeholder: "身長を入力してください", position: CGPoint(x:self.view.bounds.width/2,y:120))
        // BMI用の入力フィールドを設置(自動計算).
        myWriteBMIField.enabled = false
        self.setTextField(myWriteBMIField, placeholder: "BMIの計算値", position: CGPoint(x:self.view.bounds.width/2,y:155))
        // 体脂肪率用の入力フィールドを設置(自動計算).
        self.setTextField(myWriteBodyFatField, placeholder: "体脂肪率を入力してください", position: CGPoint(x:self.view.bounds.width/2,y:190))
        // 除脂肪体重用の入力フィールドを設置(自動計算).
        self.setTextField(myWriteLeanMassField, placeholder: "除脂肪体重を入力してください", position: CGPoint(x:self.view.bounds.width/2,y:225))
        
        // 書き込みボタンを設置.
        myWriteButton = UIButton()
        myWriteButton.frame = CGRectMake(0,0,300,40)
        myWriteButton.backgroundColor = UIColor.blueColor();
        myWriteButton.layer.masksToBounds = true
        myWriteButton.setTitle("体重の書き込み", forState: UIControlState.Normal)
        myWriteButton.setTitleColor(UIColor.whiteColor(), forState: UIControlState.Normal)
        myWriteButton.setTitleColor(UIColor.blackColor(), forState: UIControlState.Highlighted)
        myWriteButton.layer.cornerRadius = 20.0
        myWriteButton.layer.position = CGPoint(x: self.view.frame.width/2, y:280)
        myWriteButton.tag = 2
        myWriteButton.addTarget(self, action: #selector(ViewController.onClickMyButton(_:)), forControlEvents: .TouchUpInside)
        self.view.addSubview(myWriteButton);
        
        // 体重用の表示フィールドを設置.
        myReadBodyMassField.enabled = false
        self.setTextField(myReadBodyMassField, placeholder: "前回登録の体重", position: CGPoint(x:self.view.bounds.width/2,y:350))
        // 身長用の表示フィールドを設置.
        myReadHeightField.enabled = false
        self.setTextField(myReadHeightField, placeholder: "前回登録の身長", position: CGPoint(x:self.view.bounds.width/2,y:385))
        // BMI用の表示フィールドを設置.
        myReadBMIField.enabled = false
        self.setTextField(myReadBMIField, placeholder: "前回登録のBMI", position: CGPoint(x:self.view.bounds.width/2,y:420))
        // 体脂肪率用の表示フィールドを設置.
        myReadBodyFatField.enabled = false
        self.setTextField(myReadBodyFatField, placeholder: "前回登録の体脂肪率", position: CGPoint(x:self.view.bounds.width/2,y:490))
        // 除脂肪体重用の表示フィールドを設置.
        myReadLeanMassField.enabled = false
        self.setTextField(myReadLeanMassField, placeholder: "前回登録の除脂肪体重", position: CGPoint(x:self.view.bounds.width/2,y:455))
        
        // 読み込みボタンを設置.
        myReadButton = UIButton()
        myReadButton.frame = CGRectMake(0,0,300,40)
        myReadButton.backgroundColor = UIColor.redColor();
        myReadButton.layer.masksToBounds = true
        myReadButton.setTitle("体重の読み込み", forState: UIControlState.Normal)
        myReadButton.setTitleColor(UIColor.whiteColor(), forState: UIControlState.Normal)
        myReadButton.setTitleColor(UIColor.blackColor(), forState: UIControlState.Highlighted)
        myReadButton.layer.cornerRadius = 20.0
        myReadButton.layer.position = CGPoint(x: self.view.frame.width/2, y:540)
        myReadButton.tag = 1
        myReadButton.addTarget(self, action: #selector(ViewController.onClickMyButton(_:)), forControlEvents: .TouchUpInside)
        self.view.addSubview(myReadButton);
    }
    
    override func viewDidAppear(animated: Bool) {
        super.viewDidAppear(animated)
        // HealthStoreへの許可を申請.
        requestAuthorization()
    }
    
    /*
     TextFieldの初期化用関数.
     */
    private func setTextField(textField:UITextField,placeholder:String,position:CGPoint){
        
        textField.frame = CGRectMake(0,0,300,30)
        textField.placeholder = placeholder
        textField.delegate = self
        textField.borderStyle = UITextBorderStyle.RoundedRect
        textField.layer.position = position;
        
        self.view.addSubview(textField)
    }
    
    // MARK: - UITextFieldDelegate
    
    /*
     UITextFieldが編集終了する直前に呼ばれる.
     */
    func textFieldShouldEndEditing(textField: UITextField) -> Bool {
        
        // 身長・体重どちらも入力済みであったらBMIの計算を行う.
        if myWriteBodyMassField.text != "" && myWriteHeightField.text != "" {
            let myBodyMass = Double(myWriteBodyMassField.text!)
            let myHeight = Double(myWriteHeightField.text!)
            if myBodyMass != nil && myHeight != nil {
                let myBMI = myBodyMass! / ((myHeight! / 100) * (myHeight! / 100))
                myWriteBMIField.text = "\(myBMI)"
            }
        }
        return true
    }
    
    /*
     Healthデータへのアクセスを申請する関数.
     */
    private func requestAuthorization(){
        // 読み込みを許可する型.
        let types = Set(arrayLiteral:
            HKObjectType.quantityTypeForIdentifier(HKQuantityTypeIdentifierBodyMass)!,
            HKObjectType.quantityTypeForIdentifier(HKQuantityTypeIdentifierHeight)!,
            HKObjectType.quantityTypeForIdentifier(HKQuantityTypeIdentifierBodyMassIndex)!,
            HKObjectType.quantityTypeForIdentifier(HKQuantityTypeIdentifierBodyFatPercentage)!,
            HKObjectType.quantityTypeForIdentifier(HKQuantityTypeIdentifierLeanBodyMass)!
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
     ボタンイベント.
     */
    func onClickMyButton(sender: UIButton){
        if(sender.tag == 1){
            readData()
        } else if(sender.tag == 2){
            let bodyMass = Double(myWriteBodyMassField.text!)
            let height = Double(myWriteHeightField.text!)
            let bmi = Double(myWriteBMIField.text!)
            let fat = Double(myWriteBodyFatField.text!)
            let leanBodyMass = Double(myWriteLeanMassField.text!)
            
            if bodyMass != nil && height != nil && bmi != nil && fat != nil && leanBodyMass != nil {
                writeData(bodyMass!, Height: height!, BMI: bmi!, Fat: fat!, LeanMass: leanBodyMass!)
            }
        }
    }
    
    /*
     データの読み出し用の関数.
     */
    private func readData() {
        // 取得したいデータのタイプを生成する.
        let typeOfBodyMass:HKSampleType = HKSampleType.quantityTypeForIdentifier(HKQuantityTypeIdentifierBodyMass)!
        let typeOfHeight:HKSampleType = HKSampleType.quantityTypeForIdentifier(HKQuantityTypeIdentifierHeight)!
        let typeOfBMI:HKSampleType = HKSampleType.quantityTypeForIdentifier(HKQuantityTypeIdentifierBodyMassIndex)!
        let typeOfFat:HKSampleType = HKSampleType.quantityTypeForIdentifier(HKQuantityTypeIdentifierBodyFatPercentage)!
        let typeofLeanBodyMass:HKSampleType = HKSampleType.quantityTypeForIdentifier(HKQuantityTypeIdentifierLeanBodyMass)!
        
        let kg = HKUnit.gramUnitWithMetricPrefix(.Kilo)
        getBodyCompData(typeOfBodyMass, unit: kg, inputField: myReadBodyMassField)
        getBodyCompData(typeofLeanBodyMass, unit: kg, inputField: myReadLeanMassField)
        
        let cm = HKUnit.meterUnitWithMetricPrefix(.Centi)
        getBodyCompData(typeOfHeight, unit: cm, inputField: myReadHeightField)
        
        let count = HKUnit.countUnit()
        getBodyCompData(typeOfBMI, unit: count, inputField: myReadBMIField)
        
        let per = HKUnit.percentUnit()
        getBodyCompData(typeOfFat, unit: per, inputField: myReadBodyFatField)
    }
    
    /*
     データの読み出しのためのクエリを生成・発行するための関数.
     */
    private func getBodyCompData(sampletype:HKSampleType,unit:HKUnit,inputField:UITextField){
        let calendar = NSCalendar.init(identifier: NSCalendarIdentifierGregorian)
        let now = NSDate()
        let startDate = calendar!.startOfDayForDate(now)
        let endDate = calendar!.dateByAddingUnit(NSCalendarUnit.Day, value: 1, toDate: startDate, options: NSCalendarOptions.MatchFirst)
        let predicate = HKQuery.predicateForSamplesWithStartDate(startDate, endDate: endDate, options: HKQueryOptions.None)
        
        // データ取得時に登録された時間でソートするためのDescriptorを生成.
        let mySortDescriptor = NSSortDescriptor(key:HKSampleSortIdentifierStartDate, ascending: false)
        
        // 体重データ読み出しのためのqueryを生成.
        let mySampleQuery = HKSampleQuery(sampleType: sampletype, predicate: predicate, limit: 1, sortDescriptors: [mySortDescriptor])
        { (sampleQuery, results, error ) -> Void in
            // 一番最近に登録されたデータを取得.
            guard let myRecentSample = results!.first as? HKQuantitySample else {
                print("error")
                inputField.text = "Data is not found"
                return
            }
            // 取得したサンプルを単位に合わせる.
            dispatch_async(dispatch_get_main_queue(),{
                inputField.text = "\(myRecentSample.quantity)"
            })
        }
        
        // queryを発行.
        self.myHealthStore.executeQuery(mySampleQuery)
    }
    
    /*
     データの書き込み用の関数.
     */
    private func writeData(BodyMass: Double, Height: Double, BMI: Double, Fat: Double, LeanMass: Double){
        
        // 登録用データタイプを生成.
        let typeOfBodyMass:HKQuantityType = HKObjectType.quantityTypeForIdentifier(HKQuantityTypeIdentifierBodyMass)!
        let typeOfHeight:HKQuantityType = HKObjectType.quantityTypeForIdentifier(HKQuantityTypeIdentifierHeight)!
        let typeOfBMI:HKQuantityType = HKObjectType.quantityTypeForIdentifier(HKQuantityTypeIdentifierBodyMassIndex)!
        let typeOfFat:HKQuantityType = HKObjectType.quantityTypeForIdentifier(HKQuantityTypeIdentifierBodyFatPercentage)!
        let typeofLeanBodyMass:HKQuantityType = HKObjectType.quantityTypeForIdentifier(HKQuantityTypeIdentifierLeanBodyMass)!
        
        // 単位とセットのデータを生成.
        let kg:HKUnit = HKUnit.gramUnitWithMetricPrefix(.Kilo)
        let myBodyMass = HKQuantity(unit:kg, doubleValue: BodyMass)
        let myLeanBodyMass = HKQuantity(unit: kg, doubleValue: LeanMass)
        
        let cm:HKUnit = HKUnit.meterUnitWithMetricPrefix(.Centi)
        let myHeight = HKQuantity(unit: cm, doubleValue: Height)
        
        let count:HKUnit = HKUnit.countUnit()
        let myBMI = HKQuantity(unit: count , doubleValue: BMI)
        
        let per:HKUnit = HKUnit.percentUnit()
        let myFat = HKQuantity(unit: per, doubleValue: Fat)
        
        // StoreKit保存用データを作成.
        let myBodyMassData = HKQuantitySample(type:typeOfBodyMass, quantity:myBodyMass, startDate:NSDate(), endDate:NSDate())
        let myHeightData = HKQuantitySample(type: typeOfHeight, quantity:myHeight, startDate:NSDate(), endDate:NSDate())
        let myBMIData = HKQuantitySample(type: typeOfBMI, quantity:myBMI, startDate:NSDate(), endDate:NSDate())
        let myFatData = HKQuantitySample(type: typeOfFat, quantity: myFat, startDate: NSDate(), endDate: NSDate())
        let myLeanBodyMassData = HKQuantitySample(type: typeofLeanBodyMass, quantity: myLeanBodyMass, startDate: NSDate(), endDate: NSDate())

        // HealthStoreにデータを保存.
        myHealthStore.saveObjects([myBodyMassData, myHeightData, myBMIData, myFatData, myLeanBodyMassData, ]) { (success, error) in
            if let e = error {
                print("Error: \(e.localizedDescription)")
                return
            }
            print(success ? "Success" : "Failure")
        }
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
