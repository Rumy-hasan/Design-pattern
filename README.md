# Design-pattern
Alll design pattern

# Abstract Design Pattern:

## Diagram
![alt text](https://github.com/Rumy-hasan/Design-pattern/blob/main/Abstract%20factory%20Design%20pattern.png)
![alt text](https://github.com/Rumy-hasan/Design-pattern/blob/main/Abstract%20factory%20Design%20pattern-2.png)

## Implementation

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
```

## usages:

```swift
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

## Usages

```swift
let noCurrencyCode = "No Currency Code Available"

CurrencyFactory.currency(for: .greece)?.code ?? noCurrencyCode
CurrencyFactory.currency(for: .spain)?.code ?? noCurrencyCode
CurrencyFactory.currency(for: .unitedStates)?.code ?? noCurrencyCode
CurrencyFactory.currency(for: .uk)?.code ?? noCurrencyCode

```

# Adapter

the diagram collected from refactoring.guru
## Diagram
![alt text](https://github.com/Rumy-hasan/Design-pattern/blob/main/adapter.png)

## Implementation
```swift

protocol Target{
    func request() -> String
}

class Adapter: Target{
    private var adaptee: Adaptee
    
    init(_ adaptee: Adaptee) {
        self.adaptee = adaptee
    }
    
    func request() -> String {
        return "Adapter: (TRANSLATED) " + adaptee.specificRequest().reversed()
    }
}

class Adaptee{
    public func specificRequest() -> String{
        return ".eetpadA eht fo roivaheb laicepS"
    }
}

```
## Usages
```swift
class Client {
    static func someClientCode(target: Target) {
        print(target.request())
    }
}
```

# Decorator Pattern

## Diagram
![alt text](https://github.com/Rumy-hasan/Design-pattern/blob/main/decorator.png)

## Implementation

```swift
/// The base Component interface defines operations that can be altered by
/// decorators.
protocol Component {

    func operation() -> String
}

/// Concrete Components provide default implementations of the operations. There
/// might be several variations of these classes.
class ConcreteComponent: Component {

    func operation() -> String {
        return "ConcreteComponent"
    }
}

/// The base Decorator class follows the same interface as the other components.
/// The primary purpose of this class is to define the wrapping interface for
/// all concrete decorators. The default implementation of the wrapping code
/// might include a field for storing a wrapped component and the means to
/// initialize it.
class Decorator: Component {

    private var component: Component

    init(_ component: Component) {
        self.component = component
    }

    /// The Decorator delegates all work to the wrapped component.
    func operation() -> String {
        return component.operation()
    }
}

/// Concrete Decorators call the wrapped object and alter its result in some
/// way.
class ConcreteDecoratorA: Decorator {

    /// Decorators may call parent implementation of the operation, instead of
    /// calling the wrapped object directly. This approach simplifies extension
    /// of decorator classes.
    override func operation() -> String {
        return "ConcreteDecoratorA(" + super.operation() + ")"
    }
}

/// Decorators can execute their behavior either before or after the call to a
/// wrapped object.
class ConcreteDecoratorB: Decorator {

    override func operation() -> String {
        return "ConcreteDecoratorB(" + super.operation() + ")"
    }
}
```

## usages

```swift
/// The client code works with all objects using the Component interface. This
/// way it can stay independent of the concrete classes of components it works
/// with.
class Client {
    static func someClientCode(component: Component) {
        print("Result: " + component.operation())
    }
}
```
