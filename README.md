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

# Proxy design pattern

## Diagram
![alt text](https://github.com/Rumy-hasan/Design-pattern/blob/main/proxy.png)

## Implementation

```swift
/// The Subject interface declares common operations for both RealSubject and
/// the Proxy. As long as the client works with RealSubject using this
/// interface, you'll be able to pass it a proxy instead of a real subject.
protocol Subject {
    func request()
}

/// The RealSubject contains some core business logic. Usually, RealSubjects are
/// capable of doing some useful work which may also be very slow or sensitive -
/// e.g. correcting input data. A Proxy can solve these issues without any
/// changes to the RealSubject's code.
class RealSubject: Subject {
    func request() {
        print("RealSubject: Handling request.")
    }
}

/// The Proxy has an interface identical to the RealSubject.
class Proxy: Subject {

    private lazy var realSubject: RealSubject

    /// The Proxy maintains a reference to an object of the RealSubject class.
    /// It can be either lazy-loaded or passed to the Proxy by the client.
    init(_ realSubject: RealSubject) {
        self.realSubject = realSubject
    }

    /// The most common applications of the Proxy pattern are lazy loading,
    /// caching, controlling the access, logging, etc. A Proxy can perform one
    /// of these things and then, depending on the result, pass the execution to
    /// the same method in a linked RealSubject object.
    func request() {
        if (checkAccess()) {
            realSubject.request()
            logAccess()
        }
    }

    private func checkAccess() -> Bool {
        /// Some real checks should go here.
        print("Proxy: Checking access prior to firing a real request.")
        return true
    }

    private func logAccess() {
        print("Proxy: Logging the time of request.")
    }
}
```

## Usages

```swift
class Client {
    static func clientCode(subject: Subject) {
        subject.request()
    }
}
```
# Bridge Design Patterns

## Diagram
![alt text](https://github.com/Rumy-hasan/Design-pattern/blob/main/bridge.png)
![alt text](https://github.com/Rumy-hasan/Design-pattern/blob/main/bridgeCode.png)

## Implementation

```swift
/// The Abstraction defines the interface for the "control" part of the two
/// class hierarchies. It maintains a reference to an object of the
/// Implementation hierarchy and delegates all of the real work to this object.
class Abstraction {

    fileprivate var implementation: Implementation

    init(_ implementation: Implementation) {
        self.implementation = implementation
    }

    func operation() -> String {
        let operation = implementation.operationImplementation()
        return "Abstraction: Base operation with:\n" + operation
    }
}

/// You can extend the Abstraction without changing the Implementation classes.
class ExtendedAbstraction: Abstraction {

    override func operation() -> String {
        let operation = implementation.operationImplementation()
        return "ExtendedAbstraction: Extended operation with:\n" + operation
    }
}

/// You can extend the Abstraction without changing the Implementation classes.
class ExtendedAbstractionTow: Abstraction {

    override func operation() -> String {
        let operation = implementation.operationImplementation()
        return "ExtendedAbstractionTow: Extended operation with:\n" + operation
    }
}

/// The Implementation defines the interface for all implementation classes. It
/// doesn't have to match the Abstraction's interface. In fact, the two
/// interfaces can be entirely different. Typically the Implementation interface
/// provides only primitive operations, while the Abstraction defines higher-
/// level operations based on those primitives.
protocol Implementation {

    func operationImplementation() -> String
}

/// Each Concrete Implementation corresponds to a specific platform and
/// implements the Implementation interface using that platform's API.
class ConcreteImplementationA: Implementation {

    func operationImplementation() -> String {
        return "ConcreteImplementationA: Here's the result on the platform A.\n"
    }
}

class ConcreteImplementationB: Implementation {

    func operationImplementation() -> String {
        return "ConcreteImplementationB: Here's the result on the platform B\n"
    }
}

```

## Usages

```swift
/// Except for the initialization phase, where an Abstraction object gets linked
/// with a specific Implementation object, the client code should only depend on
/// the Abstraction class. This way the client code can support any abstraction-
/// implementation combination.
class Client {
    // ...
    static func someClientCode(abstraction: Abstraction) {
        print(abstraction.operation())
    }
    // ...
}
```

