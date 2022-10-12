# Design-pattern
Alll design pattern

# Abstract Design Pattern:

Diagram
![alt text](https://github.com/Rumy-hasan/Design-pattern/blob/main/Abstract%20factory%20Design%20pattern.png)
![alt text](https://github.com/Rumy-hasan/Design-pattern/blob/main/Abstract%20factory%20Design%20pattern-2.png)

```swift
// MARK: Product
protocol FirstView{}
protocol SecondView{}

class FirstViewImplementationForiOS: FirstView{
    
}

class FirstViewImplementationForMacOS: FirstView{
    
}

class SecondViewImplementationForiOS: SecondView{
    
}

class SecondViewImplementationForMacOS: SecondView{
    
}



//MAKR: Factory or Creator
protocol Factory{
    func getFirstView() -> FirstView
    func getSecondView() -> SecondView
}

class FactoryForiOS: Factory{
    func getFirstView() -> FirstView {
        return FirstViewImplementationForiOS()
    }
    func getSecondView() -> SecondView{
        return SecondViewImplementationForiOS()
    }
}

class FactoryForMacOS: Factory{
    func getFirstView() -> FirstView {
        return FirstViewImplementationForMacOS()
    }
    func getSecondView() -> SecondView{
        return SecondViewImplementationForMacOS()
    }
}

## useges:

enum Platform{
    case iOS, macOS
}

class GetFactory{
    static func getProperFactory(for platform: Platform) -> Factory{
        switch platform{
        case .iOS:
            return FactoryForiOS()
        case.macOS:
            return FactoryForMacOS()
        }
    }
}

let fac = GetFactory.getProperFactory(for: .iOS)
let view1 = fac.getFirstView()
let view2 = fac.getSecondView()

```

# Factory Method

## Diagram
![alt text](https://github.com/Rumy-hasan/Design-pattern/blob/main/factory%20Method%20Design%20pattern-3.png)


## Implementation

```swift
protocol CurrencyDescribing {
    var symbol: String { get }
    var code: String { get }
}

final class Euro: CurrencyDescribing {
    var symbol: String {
        return "€"
    }
    
    var code: String {
        return "EUR"
    }
}

final class UnitedStatesDolar: CurrencyDescribing {
    var symbol: String {
        return "$"
    }
    
    var code: String {
        return "USD"
    }
}

enum Country {
    case unitedStates
    case spain
    case uk
    case greece
}

enum CurrencyFactory {
    static func currency(for country: Country) -> CurrencyDescribing? {

        switch country {
            case .spain, .greece:
                return Euro()
            case .unitedStates:
                return UnitedStatesDolar()
            default:
                return nil
        }
        
    }
}
```

## Useges

```swift
let noCurrencyCode = "No Currency Code Available"

CurrencyFactory.currency(for: .greece)?.code ?? noCurrencyCode
CurrencyFactory.currency(for: .spain)?.code ?? noCurrencyCode
CurrencyFactory.currency(for: .unitedStates)?.code ?? noCurrencyCode
CurrencyFactory.currency(for: .uk)?.code ?? noCurrencyCode

```


