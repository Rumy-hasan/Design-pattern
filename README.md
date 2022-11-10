# Design-pattern
Alll design pattern
- Abstract Design Pattern
- Factory Method
- Adapter
- Decorator Pattern
- Proxy design pattern
- Bridge Design Patterns
- Composite Design Patterns
- Facade Design pattern
- Command Design pattern
- Iterator design pattern
- Chain of responsibility
- State pattern

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


# Composite Design Patterns

## Diagram
![alt text](https://github.com/Rumy-hasan/Design-pattern/blob/main/Screen%20Shot%202022-10-27%20at%209.19.17%20PM.png)

## Implementation

```swift
/// The base Component class declares common operations for both simple and
/// complex objects of a composition.
protocol Component {

    /// The base Component may optionally declare methods for setting and
    /// accessing a parent of the component in a tree structure. It can also
    /// provide some default implementation for these methods.
    var parent: Component? { get set }

    /// In some cases, it would be beneficial to define the child-management
    /// operations right in the base Component class. This way, you won't need
    /// to expose any concrete component classes to the client code, even during
    /// the object tree assembly. The downside is that these methods will be
    /// empty for the leaf-level components.
    func add(component: Component)
    func remove(component: Component)

    /// You can provide a method that lets the client code figure out whether a
    /// component can bear children.
    func isComposite() -> Bool

    /// The base Component may implement some default behavior or leave it to
    /// concrete classes.
    func operation() -> String
}

extension Component {

    func add(component: Component) {}
    func remove(component: Component) {}
    func isComposite() -> Bool {
        return false
    }
}

/// The Leaf class represents the end objects of a composition. A leaf can't
/// have any children.
///
/// Usually, it's the Leaf objects that do the actual work, whereas Composite
/// objects only delegate to their sub-components.
class Leaf: Component {

    var parent: Component?

    func operation() -> String {
        return "Leaf"
    }
}

/// The Composite class represents the complex components that may have
/// children. Usually, the Composite objects delegate the actual work to their
/// children and then "sum-up" the result.
class Composite: Component {

    var parent: Component?

    /// This fields contains the conponent subtree.
    private var children = [Component]()

    /// A composite object can add or remove other components (both simple or
    /// complex) to or from its child list.
    func add(component: Component) {
        var item = component
        item.parent = self
        children.append(item)
    }

    func remove(component: Component) {
        // ...
    }

    func isComposite() -> Bool {
        return true
    }

    /// The Composite executes its primary logic in a particular way. It
    /// traverses recursively through all its children, collecting and summing
    /// their results. Since the composite's children pass these calls to their
    /// children and so forth, the whole object tree is traversed as a result.
    func operation() -> String {
        let result = children.map({ $0.operation() })
        return "Branch(" + result.joined(separator: " ") + ")"
    }
}
```

## Usages

```swift
class Client {

    /// The client code works with all of the components via the base interface.
    static func someClientCode(component: Component) {
        print("Result: " + component.operation())
    }

    /// Thanks to the fact that the child-management operations are also
    /// declared in the base Component class, the client code can work with both
    /// simple or complex components.
    static func moreComplexClientCode(leftComponent: Component, rightComponent: Component) {
        if leftComponent.isComposite() {
            leftComponent.add(component: rightComponent)
        }
        print("Result: " + leftComponent.operation())
    }
}
```

# Facade Design pattern
It's a wraper or abstruction of a complex system, where client do not deal with complex system. He just deal with abstruction.

## Diagram
![alt text](https://github.com/Rumy-hasan/Design-pattern/blob/main/Screen%20Shot%202022-10-27%20at%2010.10.26%20PM.png)

## Implementation
```swift
import XCTest

/// The Facade class provides a simple interface to the complex logic of one or
/// several subsystems. The Facade delegates the client requests to the
/// appropriate objects within the subsystem. The Facade is also responsible for
/// managing their lifecycle. All of this shields the client from the undesired
/// complexity of the subsystem.
class Facade {

    private var subsystem1: Subsystem1
    private var subsystem2: Subsystem2

    /// Depending on your application's needs, you can provide the Facade with
    /// existing subsystem objects or force the Facade to create them on its
    /// own.
    init(subsystem1: Subsystem1 = Subsystem1(),
         subsystem2: Subsystem2 = Subsystem2()) {
        self.subsystem1 = subsystem1
        self.subsystem2 = subsystem2
    }

    /// The Facade's methods are convenient shortcuts to the sophisticated
    /// functionality of the subsystems. However, clients get only to a fraction
    /// of a subsystem's capabilities.
    func operation() -> String {

        var result = "Facade initializes subsystems:"
        result += " " + subsystem1.operation1()
        result += " " + subsystem2.operation1()
        result += "\n" + "Facade orders subsystems to perform the action:\n"
        result += " " + subsystem1.operationN()
        result += " " + subsystem2.operationZ()
        return result
    }
}

/// The Subsystem can accept requests either from the facade or client directly.
/// In any case, to the Subsystem, the Facade is yet another client, and it's
/// not a part of the Subsystem.
class Subsystem1 {

    func operation1() -> String {
        return "Sybsystem1: Ready!\n"
    }

    // ...

    func operationN() -> String {
        return "Sybsystem1: Go!\n"
    }
}

/// Some facades can work with multiple subsystems at the same time.
class Subsystem2 {

    func operation1() -> String {
        return "Sybsystem2: Get ready!\n"
    }

    // ...

    func operationZ() -> String {
        return "Sybsystem2: Fire!\n"
    }
}
```

## Usages

```swift
/// The client code works with complex subsystems through a simple interface
/// provided by the Facade. When a facade manages the lifecycle of the
/// subsystem, the client might not even know about the existence of the
/// subsystem. This approach lets you keep the complexity under control.
class Client {
    // ...
    static func clientCode(facade: Facade) {
        print(facade.operation())
    }
    // ...
}
```
# Command Design pattern

It encapsulate command. not the receiver or invoker.

## Diagram
![alt text](https://github.com/Rumy-hasan/Design-pattern/blob/main/Command.png)

## Implementation
```swift
protocol CommandInterface{
    func execute()
    func unExecute()
}


class ConcreetCMDInterface: CommandInterface{
    private let receiver: Receiver
    init(r: Receiver){
        self.receiver = r
    }
    
    func execute(){
        r.callToExecute()
    }
    
    func unExecute(){
        r.callToUnExecute()
    }
}


class Receiver{
    func callToExecute(){
        
    }
    
    func callToUnExecute(){
        
    }
    //AND OTHER METHODS
}
```

## Usages

```swift
//Instead of define setCMD we can inject our command into initialization.
class Invoker{
    private let cmd1: CommandInterface!
    private let cmd2: CommandInterface!
    
    func setCMD(cmd1: CommandInterface, cmd2: CommandInterface){
        self.cmd1 = cmd1
        self.cmd2 = cmd2
    }
    
    func okBtnClick(){
        cmd1.execute()
    }
    
    func goBtnClick(){
        cmd2.execute()
    }
}
```

# Iterator design pattern

## Diagram
![alt text](https://github.com/Rumy-hasan/Design-pattern/blob/main/Screen%20Shot%202022-11-07%20at%2010.39.17%20PM.png)

## Implementation

```swift
protocol Iterable{
    func getIterator() -> Iterator
}

class ConcreetIterable:Iterable{
    func getIterator() -> Iterator{
        return ConcreetIterator(self)
    }
}

protocol Iterator{
    func hasNext()-> bool
    func traverseToNext()
    func getCurrent()
}

class ConcreetIterator: Iterator{
    private (set) let iterable:ConcreetIterable!
    init(iterable: ConcreetIterable){
        self.iterable = iterable
    }
}

extension ConcreetIterator{
    func hasNext()-> bool{
        if iterable.next != null{return true}
    }
    func traverseToNext(){
        iterable.gonext()
    }
    func getCurrent(){
        return iterable.currentItem
    }
}
```

# Chain of responsibility

## Diagram
![alt text](https://github.com/Rumy-hasan/Design-pattern/blob/main/Chain.png)

## Implementation

```swift
protocol Handler: class {

    @discardableResult
    func setNext(handler: Handler) -> Handler

    func handle(request: String) -> String?

    var nextHandler: Handler? { get set }
}

extension Handler {

    func setNext(handler: Handler) -> Handler {
        self.nextHandler = handler

        /// Returning a handler from here will let us link handlers in a
        /// convenient way like this:
        /// monkey.setNext(handler: squirrel).setNext(handler: dog)
        return handler
    }

    func handle(request: String) -> String? {
        return nextHandler?.handle(request: request)
    }
}

/// All Concrete Handlers either handle a request or pass it to the next handler
/// in the chain.
class MonkeyHandler: Handler {

    var nextHandler: Handler?

    func handle(request: String) -> String? {
        if (request == "Banana") {
            return "Monkey: I'll eat the " + request + ".\n"
        } else {
            return nextHandler?.handle(request: request)
        }
    }
}

class SquirrelHandler: Handler {

    var nextHandler: Handler?

    func handle(request: String) -> String? {

        if (request == "Nut") {
            return "Squirrel: I'll eat the " + request + ".\n"
        } else {
            return nextHandler?.handle(request: request)
        }
    }
}

class DogHandler: Handler {

    var nextHandler: Handler?

    func handle(request: String) -> String? {
        if (request == "MeatBall") {
            return "Dog: I'll eat the " + request + ".\n"
        } else {
            return nextHandler?.handle(request: request)
        }
    }
}

/// The client code is usually suited to work with a single handler. In most
/// cases, it is not even aware that the handler is part of a chain.
class Client {
    // ...
    static func someClientCode(handler: Handler) {

        let food = ["Nut", "Banana", "Cup of coffee"]

        for item in food {

            print("Client: Who wants a " + item + "?\n")

            guard let result = handler.handle(request: item) else {
                print("  " + item + " was left untouched.\n")
                return
            }

            print("  " + result)
        }
    }
    // ...
}
```
## Usages

```swift
/// Let's see how it all works together.
class ChainOfResponsibilityConceptual: XCTestCase {
 
    func test() {

        /// The other part of the client code constructs the actual chain.

        let monkey = MonkeyHandler()
        let squirrel = SquirrelHandler()
        let dog = DogHandler()
        monkey.setNext(handler: squirrel).setNext(handler: dog)

        /// The client should be able to send a request to any handler, not just
        /// the first one in the chain.

        print("Chain: Monkey > Squirrel > Dog\n\n")
        Client.someClientCode(handler: monkey)
        print()
        print("Subchain: Squirrel > Dog\n\n")
        Client.someClientCode(handler: squirrel)
    }
}
```

# State pattern

## Diagram
![alt text](https://github.com/Rumy-hasan/Design-pattern/blob/main/state.png)

## Implementation

```swift
class Context {

    /// A reference to the current state of the Context.
    private var state: State

    init(_ state: State) {
        self.state = state
        transitionTo(state: state)
    }

    /// The Context allows changing the State object at runtime.
    func transitionTo(state: State) {
        print("Context: Transition to " + String(describing: state))
        self.state = state
        self.state.update(context: self)
    }

    /// The Context delegates part of its behavior to the current State object.
    func request1() {
        state.handle1()
    }

    func request2() {
        state.handle2()
    }
}

/// The base State class declares methods that all Concrete State should
/// implement and also provides a backreference to the Context object,
/// associated with the State. This backreference can be used by States to
/// transition the Context to another State.
protocol State: class {

    func update(context: Context)

    func handle1()
    func handle2()
}

class BaseState: State {

    private(set) weak var context: Context?

    func update(context: Context) {
        self.context = context
    }

    func handle1() {}
    func handle2() {}
}

/// Concrete States implement various behaviors, associated with a state of the
/// Context.
class ConcreteStateA: BaseState {

    override func handle1() {
        print("ConcreteStateA handles request1.")
        print("ConcreteStateA wants to change the state of the context.\n")
        context?.transitionTo(state: ConcreteStateB())
    }

    override func handle2() {
        print("ConcreteStateA handles request2.\n")
    }
}

class ConcreteStateB: BaseState {

    override func handle1() {
        print("ConcreteStateB handles request1.\n")
    }

    override func handle2() {
        print("ConcreteStateB handles request2.")
        print("ConcreteStateB wants to change the state of the context.\n")
        context?.transitionTo(state: ConcreteStateA())
    }
}
```

```swift
/// Let's see how it all works together.
class StateConceptual: XCTestCase {

    func test() {
        let context = Context(ConcreteStateA())
        context.request1()
        context.request2()
    }
}
```
