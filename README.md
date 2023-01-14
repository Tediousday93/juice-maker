# 🧃Juice Maker

## 📚목차
1. [소개](#1-소개)
2. [팀원](#2-팀원)
3. [타임라인](#3-타임라인)
4. [프로젝트 구조](#4-프로젝트-구조)
5. [실행화면(기능 설명)](#5-실행-화면기능-설명)
6. [트러블슈팅](#6-트러블-슈팅)
7. [참고링크](#7-참고-링크)

<br/>

## 1. 소개
생과일 쥬스를 만드는 객체와 과일저장소 객체의 상호작용을 통해 과일의 재고를 관리하는 프로그램

### **Step01**
* **Model 구현**

    - **FruitStore class**
      딸기, 바나나, 파인애플, 키위, 망고 5종류의 과일 재고를 관리하기 위한 클래스 정의
      <br/>
      > * 각 과일의 초기 재고: 10개
      > * 각 과일의 수량 n개를 변경하는 메서드
      > * 각 과일의 재고를 확인하는 메서드
    - **JuiceMaker struct**
      FruitStore의 과일을 사용해 과일을 제조하는 구조체 정의
      <br/>
      > * 각 쥬스 별 들어가는 과일의 개수를 기록하는 Nested Type Juice 열거체
      > * 쥬스를 만드는 make 메서드

### **Step02**
* **View Controller 구현**

    - **MainViewController**
        쥬스 주문 화면을 관리하는 ViewController 정의
        <br/>
        > * 각 과일의 재고를 표시하는 Label
        > * 쥬스 주문 버튼의 ```@IBOutlet```, ```@IBAction```
        > * 화면이 Load되었을 때, Label Text를 초기화하는 메서드
        > * 재고가 부족할 때 띄워줄 Alert
    <br/>
    
    - **Modal**
        ```MainViewController```와 ```ChangeStockViewController```의 화면 전환 구현 
        <br/>
        > * Modal 방식을 통한 화면전환 구현 
        > * Alert을 통한 화면전환
        > * Button을 통한 화면전환

<br/>

## 2. 팀원


| 송준 | Rowan |
| :--------: | :--------:| 
|   <Img src = "https://i.imgur.com/9Bd6NIT.png" width="200" height="200"/>  |  <Img src = "https://i.imgur.com/NP7PZ6m.png" width="200" height="200"/> |
| <center>Driver, Navigator</center>  | <center>Driver, Navigator</center>     
| <center>[Github Profile](https://github.com/kimseongj)</center> | <center>[Github Profile](https://github.com/Kyeongjun2)</center> |



</br>

## 3. 타임라인
**프로젝트 진행 기간** 
- **23.01.02 (월) ~ 23.01.20 (금)** 

> 23.01.02 (월) : 참고자료 읽기, Model 기본 객체 및 메서드 정의 <br/>
> 23.01.03 (화) : 변수명 / 함수 리팩토링, PR 보내기 <br/>
> 23.01.04 (수) : PR 피드백을 통한 코드 리팩토링 <br/>
> 23.01.05 (목) : STEP 1. 머지 / STEP 2. 사전학습 <br/>
> 23.01.06 (금) : STEP 2. 진행 (뷰컨트롤러 정의, 화면전환 구현) <br/>
> 23.01.09 (월) : STEP 2. 화면 구성, 변경 사항 전달을 위한 싱글턴 패턴 적용 <br/>
> 23.01.10 (화) : STEP 2. refactoring(컨벤션 수정, 주문 버튼 ```@IBAction``` 통합) <br/>
> 23.01.11 (수) : STEP 2. 머지 / STEP 3. 사전학습 <br/>

<br/>

## 4. 프로젝트 구조
~~3주차 추가 예정~~
<br/>

## 5. 실행 화면(기능 설명)
#### ◾️ 쥬스 주문 버튼 (성공)
![쥬스메이커버튼클릭](https://user-images.githubusercontent.com/88870642/212242601-a006c81c-e6b1-4020-a8ab-e62be3c00c24.gif)
- 각 쥬스 주문 버튼을 누르면 과일 하단 Label에 쥬스의 재료만큼 감소한 재고 변동사항을 반영

#### ◾️ 쥬스 주문 버튼 (실패)  
![쥬스메이커-얼럿](https://user-images.githubusercontent.com/88870642/212242609-0163ee2a-f9ec-42ca-9ad9-7763f8166c0a.gif)
- 재고가 모자랄 경우, 주문 버튼을 누르면 재고 수정 화면으로 이동할 수 있도록 Alert를 띄워줌

<br/>

## 6. 트러블 슈팅
### STEP 1.

<details>
<summary>과일 재고 표현 및 초기값 할당</summary>

- 3단계에 걸쳐 과일 재고를 표현하는 방법을 개선하였습니다.
#### 1. 과일 클래스를 인스턴스화해서 과일/개수 할당
- 재고 변경 시, 각각의 과일 인스턴스를 참조하는 방법이 까다로웠습니다. 빈 배열을 하나 만들어서 각각의 과일 인스턴스들을 배열에 넣어주었고 재고 변경을 원하는 과일을 찾을 수 있도록 findFruit 메서드를 추가로 정의하였습니다.

```swift
class FruitStore {
    class Fruit {
        let name: String
        let stock = 10
        
        init(name: String) {
            self.name = name
        }
    }

    let strawberry = Fruit(name: "딸기")
    let banana = Fruit(name: "바나나")
    let kiwi = Fruit(name: "키위")
    let pineapple = Fruit(name: "파인애플")
    let mango = Fruit(name: "망고")

    func makeFruitsList() -> [Fruit] {
        let fruitsList = [self.strawberry, self.banana, self.kiwi, self.pineapple, self.mango]
        return fruitsList
    }

    func findFruit(fruitName: String) -> Fruit? {
        for i in 0..<makeFruitsList().count {
            if makeFruitsList()[i].name == fruitName {
                return makeFruitsList()[i]
            }
        }
    }
}
```

<br/>

#### 2. Dictionary[String: Int]로 과일/개수 할당
- 과일 클래스 인스턴스를 메서드를 통해 Array에 담아주는 방법이 불편하고, 과일 인스턴스 프로퍼티에 재고를 할당하여 재고관리에 어려움을 느꼈습니다.
- 좀 더 간결한 표현과 재고 접근의 용이성을 위해 Array에서 Dictionary로 재고 표현 방법을 변경하였습니다.
```swift
class FruitStore {
    var fruitsStock = ["딸기":10, "망고":10, "바나나":10, "키위":10, "파인애플":10]

    func subtractInventory(fruit: String, number: Int) throws {
        guard let selectedStock = self.fruitsStock[fruit],
              selectedStock - number >= 0 else {
            throw StockError.outOfStock
        }
        
        fruitsStock[fruit] = selectedStock - number
    }
    
    func addInventory(fruit: String, number: Int) {
        if let selectedStock = self.fruitsStock[fruit] {
            fruitsStock[fruit] = selectedStock + number
        }
    }
    
    func checkStock(fruit: String) {
            let resultMessage = "\(fruit)의 재고는 \(fruitsStock[fruit])개입니다."
}
```
<br/>

#### 3. Dictionary[Fruits: Int]로 과일/개수 할당
- Dictionary Key값이 String일 경우 Key값의 범위가 너무 넓어 명시적이지 않습니다.
- 열거형 타입으로 Key값의 범위를 한정하여 가독성과 타입의 명시성을 높였습니다.

```swift
enum Fruits: CaseIterable {
    case strawberry
    case mango
    case pineapple
    case kiwi
    case banana
}

var fruitsStock = [Fruits: Int]()
    
    func fillFruitsStock() {
        for fruit in Fruits.allCases {
            fruitsStock[fruit] = 10
        }
    }
    
    init() {
        self.fillFruitsStock()
    }
}
```
<br/>

</details>

### STEP2.

<details>
<summary>값 타입? 참조 타입?</summary>

#### 수정 전
- 싱글톤 형태인 ```FruitStore.shared.fruitsStock``` 프로퍼티를 ```fruitsStock```변수로 선언했습니다. 
- ```FruitStore.shared```의 경우 class로 참조타입이지만, ```FruitStore.shared.fruitsStock```은 일반 프로퍼티로 값타입입니다. 값타입 프로퍼티를 새로운 변수에 할당했기 때문에 재고 변경 시 값의 복사가 일어났습니다. 
- **Copy on Write**에 의해 변경되기 전까지 같은 메모리를 참조하지만, 저희 프로그램의 경우 ```fruitsStock```의 변경이 일어나기 때문에 싱글톤 인스턴스의 프로퍼티를 참조할 수 없는 문제점을 발견했습니다.
```swift
// 수정 전
final class ViewController: UIViewController {
    private let juiceMaker = JuiceMaker()
    private var fruitsStock = FruitStore.shared.fruitsStock
    
    override func viewDidLoad() {
        super.viewDidLoad()
        displayStock()
    }
    
    func displayStock() {
        if let strawberryStock = fruitsStock[.strawberry],
           ... {
            stockOfStrawberry.text = String(strawberryStock)
            ...
        }
    }
    
    @IBAction func orderStrawberryBananaJuice(_ sender: UIButton) {
        do {
            try juiceMaker.make(juice: .strawberryBanana)
            displayStock()
        } catch ... 
    }
}
```
#### 수정 후
- 위의 문제점을 해결하기 위해 연산 프로퍼티를 사용하였습니다.
- 연산 프로퍼티의 특징은 연산 실제 값을 수정하는 것이 아닌 ```getter```와 ```setter```를 통해 다른 속성과 값을 간접적으로 읽고, 쓸 수 있습니다.
- 연산 프로퍼티를 통해 ```FruitStore.shared.fruitsStock```을 직접 반환해주는 코드를 구현했습니다.
```swift
// 수정 후
final class ViewController: UIViewController {
    private let juiceMaker = JuiceMaker()
    private var fruitsStock: [Fruits: Int] {
        return FruitStore.shared.fruitsStock
    } 
    
    override func viewDidLoad() {
        super.viewDidLoad()
        displayStock()
    }
    
    func displayStock() {
        if let strawberryStock = fruitsStock[.strawberry],
           ... {
            stockOfStrawberry.text = String(strawberryStock)
            ...
        }
    }
    
    @IBAction func orderStrawberryBananaJuice(_ sender: UIButton) {
        do {
            try juiceMaker.make(juice: .strawberryBanana)
            displayStock()
        } catch ... 
    }
}
```
<br/>
</details>

<details>
<summary>화면전환 방법</summary>

#### 변경 전
- ```self.navigationController?.pushViewController(nextVC, animated: true) ```를 통해 화면 전환 시 Navigation 방식으로 화면전환이 되었습니다.

```swift
func moveToChangeStockViewController() {
    guard let nextVC = self.storyboard?.instantiateViewController(withIdentifier: "ChangeStock") as? ChangeStockViewController else { return }
    self.navigationController?.pushViewController(nextVC, animated: true)  
}
```

#### 변경 후
- Navigation 방식의 경우 정보의 깊이가 깊어질 경우 사용하는 것인데, 재고 변경 화면은 기존의 쥬스를 주문하던 화면과는 다른 맥락이라고 생각하여 Modal 방식을 채택했습니다. 
- ```pushViewController```를 ```present```로 변경했습니다.
```swift
func moveToChangeStockViewController() {
    guard let nextVC = self.storyboard?.instantiateViewController(withIdentifier: "ChangeStock") as? ChangeStockViewController else { return }
    self.navigationController?.present(nextVC, animated: true)
}
```

<br/>
</details>

<details>
<summary>주문 버튼의 @IBAction 통합</summary>

#### 수정 전
* 주문 버튼 각각의 ```@IBAction```을 정의하여 7개의 같은 코드가 있었습니다.
```swift
@IBAction func orderStrawberryBananaJuice(_ sender: UIButton) {
    do {
        try juiceMaker.make(juice: .strawberryBanana)
        setSuccessAlert(juice: .strawberryBanana)
        displayStock()
    } catch StockError.outOfStock {
        setFailAlert()
    } catch {
        print(error)
    }
}

@IBAction func orderPineappleJuice(_ sender: UIButton) {
    do {
        try juiceMaker.make(juice: .pineapple)
        setSuccessAlert(juice: .pineapple)
        displayStock()
    } catch StockError.outOfStock {
        setFailAlert()
    } catch {
        print(error)
    }
}

@IBAction func orderKiwiJuice(_ sender: UIButton) {
    // 위와 동일, juice == .kiwi
}

@IBAction func orderBananaJuice(_ sender: UIButton) {
    // 위와 동일, juice == .banana
}
...
```

<br/>

#### 수정 후
* sender를 구분하는 메서드(```identifyJuice(of:)```)와 ```order(_:)```메서드를 추가하여 ```@IBAction```을 하나만 정의하고 7개의 버튼을 해당 Action에 연결했습니다.
* 본 트러블슈팅을 통해 반복된 코드를 현저히 줄일 수 있었습니다.
```swift
@IBAction func pushOrderButton(_ sender: UIButton) {
    guard let selectedJuice = identifyJuice(of: sender) else { return }

    order(selectedJuice)
}

func identifyJuice(of button: UIButton) -> JuiceMaker.Juice? {
    switch button {
    case orderStrawberry: return .strawberry
    case orderBanana: return .banana
    case orderPineapple: return .pineapple
    case orderKiwi: return .kiwi
    case orderMango: return .mango
    case orderStrawberryBanana: return .strawberryBanana
    case orderMangoKiwi: return .mangoKiwi
    default: return nil
    }
}

 func order(_ juice: JuiceMaker.Juice) {
    do {
        try juiceMaker.make(juice: juice)
        setSuccessAlert(juice: juice)
        displayStock()
    } catch StockError.outOfStock {
        setFailAlert()
    } catch {
        print(error)
    }
}
```
</details>


## 7. 참고 링크
- [Nested Type - Swift Language Guide](https://docs.swift.org/swift-book/LanguageGuide/NestedTypes.html)
- [Properties; Computed Property - Swift Language Guide](https://docs.swift.org/swift-book/LanguageGuide/Properties.html)
- [NavigationController - Swift Language Guide](https://developer.apple.com/documentation/uikit/uinavigationcontroller)
- [Type Casting - Swift Language Guide](https://docs.swift.org/swift-book/LanguageGuide/TypeCasting.html )
- [Error Handling - Swift Language Guide](https://docs.swift.org/swift-book/LanguageGuide/ErrorHandling.html)
<br/>




