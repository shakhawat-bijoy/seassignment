# Design Patterns: Structural and Behavioral Patterns
## Comprehensive Study Report

---

### Table of Contents

**1. Introduction**
**2. Structural Design Patterns**
   - 2.1 Adapter Pattern
   - 2.2 Composite Pattern
   - 2.3 Proxy Pattern
   - 2.4 Flyweight Pattern
   - 2.5 Facade Pattern
   - 2.6 Bridge Pattern
   - 2.7 Decorator Pattern

**3. Behavioral Design Patterns**
   - 3.1 Template Method Pattern
   - 3.2 Mediator Pattern
   - 3.3 Chain of Responsibility Pattern
   - 3.4 Observer Pattern
   - 3.5 Strategy Pattern
   - 3.6 Command Pattern
   - 3.7 State Pattern
   - 3.8 Visitor Pattern
   - 3.9 Iterator Pattern
   - 3.10 Interpreter Pattern
   - 3.11 Memento Pattern

**4. Conclusion**

---

## 1. Introduction

Design patterns are reusable solutions to commonly occurring problems in software design. They represent best practices evolved over time and provide a common vocabulary for developers. This report covers Structural and Behavioral design patterns, which are two of the three main categories of design patterns identified by the Gang of Four (GoF).

**Structural Patterns** deal with object composition and relationships between entities, focusing on how classes and objects can be combined to form larger structures.

**Behavioral Patterns** are concerned with algorithms and the assignment of responsibilities between objects, focusing on communication patterns and the flow of control.

---

## 2. Structural Design Patterns

### 2.1 Adapter Pattern

**Theory:**
The Adapter pattern allows incompatible interfaces to work together. It acts as a bridge between two incompatible interfaces by wrapping an existing class with a new interface. This pattern is useful when you want to use an existing class with an incompatible interface.

**UML Diagram:**
```
    Client  ────────►  Target
                         ↑
                         │
                      Adapter  ────────►  Adaptee
                         │                   │
                         └─── delegates ────┘
```

**Structure:**
- **Target**: The interface that clients expect
- **Adapter**: Adapts the Adaptee interface to the Target interface
- **Adaptee**: The existing interface that needs adapting
- **Client**: Uses the Target interface

**Partial Code Example:**
```java
// Target interface
interface MediaPlayer {
    void play(String audioType, String fileName);
}

---

### 3.8 Visitor Pattern

**Theory:**
The Visitor pattern lets you define new operations without changing the classes of the elements on which it operates. It separates algorithms from the objects on which they operate by moving the operational logic into separate visitor classes.

**UML Diagram:**
```
    Client ──────► Visitor ◄───── ConcreteVisitor
                      ↑               │
                      │            visit()
    Element ──────► AcceptVisitor     │
       ↑                 │            │
       │              accept() ◄──────┘
   ConcreteElement
```

**Structure:**
- **Visitor**: Interface declaring visit operations for each ConcreteElement
- **ConcreteVisitor**: Implements operations defined by Visitor
- **Element**: Interface defining accept method
- **ConcreteElement**: Implements accept method and defines entry point

**Partial Code Example:**
```java
// Visitor interface
interface ShapeVisitor {
    void visit(Circle circle);
    void visit(Rectangle rectangle);
    void visit(Triangle triangle);
}

// Element interface
interface Shape {
    void accept(ShapeVisitor visitor);
}

// Concrete Elements
class Circle implements Shape {
    private double radius;
    
    public Circle(double radius) {
        this.radius = radius;
    }
    
    public double getRadius() { return radius; }
    
    @Override
    public void accept(ShapeVisitor visitor) {
        visitor.visit(this);
    }
}

class Rectangle implements Shape {
    private double width, height;
    
    public Rectangle(double width, double height) {
        this.width = width;
        this.height = height;
    }
    
    public double getWidth() { return width; }
    public double getHeight() { return height; }
    
    @Override
    public void accept(ShapeVisitor visitor) {
        visitor.visit(this);
    }
}

// Concrete Visitors
class AreaCalculator implements ShapeVisitor {
    private double totalArea = 0;
    
    @Override
    public void visit(Circle circle) {
        double area = Math.PI * circle.getRadius() * circle.getRadius();
        System.out.println("Circle area: " + area);
        totalArea += area;
    }
    
    @Override
    public void visit(Rectangle rectangle) {
        double area = rectangle.getWidth() * rectangle.getHeight();
        System.out.println("Rectangle area: " + area);
        totalArea += area;
    }
    
    @Override
    public void visit(Triangle triangle) {
        // Implementation for triangle area calculation
    }
    
    public double getTotalArea() { return totalArea; }
}

class PerimeterCalculator implements ShapeVisitor {
    @Override
    public void visit(Circle circle) {
        double perimeter = 2 * Math.PI * circle.getRadius();
        System.out.println("Circle perimeter: " + perimeter);
    }
    
    @Override
    public void visit(Rectangle rectangle) {
        double perimeter = 2 * (rectangle.getWidth() + rectangle.getHeight());
        System.out.println("Rectangle perimeter: " + perimeter);
    }
    
    @Override
    public void visit(Triangle triangle) {
        // Implementation for triangle perimeter calculation
    }
}
```

---

### 3.9 Iterator Pattern

**Theory:**
The Iterator pattern provides a way to access elements of a collection sequentially without exposing its underlying representation. It decouples algorithms from containers and provides a uniform interface for traversing different collection types.

**UML Diagram:**
```
    Client ──────► Iterator ◄───── ConcreteIterator
                      ↑                  │
                   hasNext()             │
                   next()                │
                                         │
    Aggregate ──────► ConcreteAggregate ─┘
       ↑                    │
   createIterator()    createIterator()
```

**Structure:**
- **Iterator**: Interface for accessing and traversing elements
- **ConcreteIterator**: Implements Iterator interface and tracks current position
- **Aggregate**: Interface for creating Iterator objects
- **ConcreteAggregate**: Implements Aggregate interface

**Partial Code Example:**
```java
// Iterator interface
interface Iterator<T> {
    boolean hasNext();
    T next();
}

// Aggregate interface
interface Container<T> {
    Iterator<T> getIterator();
}

// Concrete Iterator
class BookIterator implements Iterator<Book> {
    private Book[] books;
    private int position = 0;
    
    public BookIterator(Book[] books) {
        this.books = books;
    }
    
    @Override
    public boolean hasNext() {
        return position < books.length && books[position] != null;
    }
    
    @Override
    public Book next() {
        if(hasNext()) {
            return books[position++];
        }
        return null;
    }
}

// Concrete Aggregate
class BookCollection implements Container<Book> {
    private Book[] books;
    private int count = 0;
    
    public BookCollection(int maxSize) {
        books = new Book[maxSize];
    }
    
    public void addBook(Book book) {
        if(count < books.length) {
            books[count++] = book;
        }
    }
    
    @Override
    public Iterator<Book> getIterator() {
        return new BookIterator(books);
    }
}

class Book {
    private String title;
    private String author;
    
    public Book(String title, String author) {
        this.title = title;
        this.author = author;
    }
    
    // getters...
    public String getTitle() { return title; }
    public String getAuthor() { return author; }
}
```

---

### 3.10 Interpreter Pattern

**Theory:**
The Interpreter pattern defines a representation for a language's grammar and provides an interpreter to deal with this grammar. It's used to evaluate sentences in a language by representing grammar rules as classes and building an abstract syntax tree.

**UML Diagram:**
```
    Client ──────► Context
       │              │
       │              ▼
       └──────► AbstractExpression
                       ↑
                  ┌────┴────┐
                  │         │
            TerminalExpression  NonterminalExpression
                                       │
                                  expressions[]
```

**Structure:**
- **AbstractExpression**: Interface for executing operations
- **TerminalExpression**: Implements operations for terminal symbols
- **NonterminalExpression**: Implements operations for nonterminal symbols
- **Context**: Contains information global to interpreter

**Partial Code Example:**
```java
// Abstract Expression
interface Expression {
    boolean interpret(Context context);
}

// Context
class Context {
    private Map<String, Boolean> variables = new HashMap<>();
    
    public void setVariable(String name, boolean value) {
        variables.put(name, value);
    }
    
    public boolean getVariable(String name) {
        return variables.getOrDefault(name, false);
    }
}

// Terminal Expression
class VariableExpression implements Expression {
    private String variableName;
    
    public VariableExpression(String variableName) {
        this.variableName = variableName;
    }
    
    @Override
    public boolean interpret(Context context) {
        return context.getVariable(variableName);
    }
}

// Nonterminal Expressions
class AndExpression implements Expression {
    private Expression leftExpression;
    private Expression rightExpression;
    
    public AndExpression(Expression left, Expression right) {
        this.leftExpression = left;
        this.rightExpression = right;
    }
    
    @Override
    public boolean interpret(Context context) {
        return leftExpression.interpret(context) && rightExpression.interpret(context);
    }
}

class OrExpression implements Expression {
    private Expression leftExpression;
    private Expression rightExpression;
    
    public OrExpression(Expression left, Expression right) {
        this.leftExpression = left;
        this.rightExpression = right;
    }
    
    @Override
    public boolean interpret(Context context) {
        return leftExpression.interpret(context) || rightExpression.interpret(context);
    }
}

class NotExpression implements Expression {
    private Expression expression;
    
    public NotExpression(Expression expression) {
        this.expression = expression;
    }
    
    @Override
    public boolean interpret(Context context) {
        return !expression.interpret(context);
    }
}
```

---

### 3.11 Memento Pattern

**Theory:**
The Memento pattern provides the ability to restore an object to its previous state without revealing the details of its implementation. It captures and externalizes an object's internal state so that the object can be restored to this state later.

**UML Diagram:**
```
    Originator ──────► Memento
       │                 ↑
       │                 │
    createMemento()      │
    restoreMemento()     │
       │                 │
       └─────────────────┘
            │
            ▼
        Caretaker
```

**Structure:**
- **Originator**: Creates memento and uses it to restore its state
- **Memento**: Stores internal state of Originator
- **Caretaker**: Responsible for memento's safekeeping, never operates on memento

**Partial Code Example:**
```java
// Memento class
class TextMemento {
    private final String content;
    private final int cursorPosition;
    private final long timestamp;
    
    public TextMemento(String content, int cursorPosition) {
        this.content = content;
        this.cursorPosition = cursorPosition;
        this.timestamp = System.currentTimeMillis();
    }
    
    public String getContent() { return content; }
    public int getCursorPosition() { return cursorPosition; }
    public long getTimestamp() { return timestamp; }
}

// Originator
class TextEditor {
    private StringBuilder content;
    private int cursorPosition;
    
    public TextEditor() {
        this.content = new StringBuilder();
        this.cursorPosition = 0;
    }
    
    public void write(String text) {
        content.append(text);
        cursorPosition += text.length();
    }
    
    public void setCursor(int position) {
        if(position >= 0 && position <= content.length()) {
            this.cursorPosition = position;
        }
    }
    
    // Create Memento
    public TextMemento save() {
        return new TextMemento(content.toString(), cursorPosition);
    }
    
    // Restore from Memento
    public void restore(TextMemento memento) {
        this.content = new StringBuilder(memento.getContent());
        this.cursorPosition = memento.getCursorPosition();
    }
    
    public String getContent() { return content.toString(); }
    public int getCursorPosition() { return cursorPosition; }
}

// Caretaker
class EditorHistory {
    private Stack<TextMemento> history = new Stack<>();
    private TextEditor editor;
    
    public EditorHistory(TextEditor editor) {
        this.editor = editor;
    }
    
    public void backup() {
        history.push(editor.save());
    }
    
    public void undo() {
        if(!history.isEmpty()) {
            TextMemento memento = history.pop();
            editor.restore(memento);
        }
    }
    
    public boolean canUndo() {
        return !history.isEmpty();
    }
    
    public void showHistory() {
        System.out.println("History has " + history.size() + " states");
        for(int i = 0; i < history.size(); i++) {
            TextMemento memento = history.get(i);
            System.out.println("State " + i + ": " + memento.getContent().substring(0, 
                Math.min(20, memento.getContent().length())) + "...");
        }
    }
}
```

---

## 4. Conclusion

Design patterns are fundamental tools in software engineering that provide proven solutions to recurring design problems. This comprehensive study of Structural and Behavioral patterns demonstrates their importance in creating maintainable, flexible, and robust software systems.

**Key Benefits of Design Patterns:**

**Structural Patterns** help organize code by defining relationships between entities. They promote code reusability through composition and delegation rather than inheritance, making systems more flexible and easier to maintain. The Adapter pattern enables integration of incompatible systems, Composite simplifies hierarchical structures, Proxy provides controlled access, Flyweight optimizes memory usage, Facade simplifies complex interfaces, Bridge separates abstraction from implementation, and Decorator adds functionality dynamically.

**Behavioral Patterns** focus on communication between objects and the assignment of responsibilities. They help distribute behavior across objects in ways that increase flexibility in carrying out communication. Template Method provides algorithmic frameworks, Mediator reduces coupling between communicating objects, Chain of Responsibility decouples senders and receivers, Observer enables loose coupling in event systems, Strategy makes algorithms interchangeable, Command encapsulates requests as objects, State enables state-dependent behavior, Visitor separates algorithms from data structures, Iterator provides uniform traversal, Interpreter enables language processing, and Memento enables state restoration.

**Best Practices for Implementation:**

1. **Choose Appropriate Patterns**: Not every problem requires a design pattern. Use patterns when they solve actual problems, not just because they exist.

2. **Understand Trade-offs**: Each pattern has benefits and costs. Consider complexity, performance, and maintainability implications.

3. **Combine Patterns Wisely**: Patterns often work together. For example, Abstract Factory with Strategy, or Observer with Mediator.

4. **Avoid Over-engineering**: Start simple and refactor toward patterns when the need becomes clear.

5. **Document Pattern Usage**: Make it clear when and why patterns are used in your codebase.

Design patterns represent decades of collective experience in software development. They provide a shared vocabulary that enables developers to communicate complex design concepts efficiently. By understanding and applying these patterns appropriately, developers can create more robust, maintainable, and scalable software systems.

The patterns covered in this report form the foundation of many modern software architectures and frameworks. Mastering these patterns is essential for any software developer who wants to write high-quality, professional code that stands the test of time.

---

**References:**
- Gamma, E., Helm, R., Johnson, R., & Vlissides, J. (1994). Design Patterns: Elements of Reusable Object-Oriented Software. Addison-Wesley.
- Freeman, E., & Robson, E. (2020). Head First Design Patterns. O'Reilly Media.
- Shvets, A. (2019). Dive Into Design Patterns. Refactoring.Guru.

---

*This report provides a comprehensive overview of 18 essential design patterns with theoretical foundations, structural diagrams, and practical code examples. Each pattern includes partial implementations that demonstrate core concepts while maintaining focus on understanding rather than complete implementation details.*

// Adaptee - existing incompatible interface
class AdvancedMediaPlayer {
    void playVlc(String fileName) { /* implementation */ }
    void playMp4(String fileName) { /* implementation */ }
}

// Adapter class
class MediaAdapter implements MediaPlayer {
    AdvancedMediaPlayer advancedPlayer;
    
    public MediaAdapter(String audioType) {
        // Initialize based on audio type
        advancedPlayer = new AdvancedMediaPlayer();
    }
    
    @Override
    public void play(String audioType, String fileName) {
        if(audioType.equalsIgnoreCase("vlc")) {
            advancedPlayer.playVlc(fileName);
        }
        // ... other adaptations
    }
}
```

---

### 2.2 Composite Pattern

**Theory:**
The Composite pattern allows you to compose objects into tree structures to represent part-whole hierarchies. It lets clients treat individual objects and compositions of objects uniformly. This pattern is ideal for representing hierarchical structures like file systems, organizational charts, or UI components.

**UML Diagram:**
```
         Component
            ↑
    ┌───────┴───────┐
    │               │
   Leaf         Composite
                    │
                    └─── contains ───► Component
```

**Structure:**
- **Component**: Abstract base class defining common interface
- **Leaf**: Represents leaf objects with no children
- **Composite**: Defines behavior for components having children

**Partial Code Example:**
```java
// Component interface
abstract class FileSystemComponent {
    protected String name;
    
    public abstract void display(int depth);
    public abstract int getSize();
    
    // Default implementations for composite operations
    public void add(FileSystemComponent component) {
        throw new UnsupportedOperationException();
    }
    
    public void remove(FileSystemComponent component) {
        throw new UnsupportedOperationException();
    }
}

// Leaf
class File extends FileSystemComponent {
    private int size;
    
    public File(String name, int size) {
        this.name = name;
        this.size = size;
    }
    
    @Override
    public void display(int depth) {
        System.out.println("-".repeat(depth) + name);
    }
    
    @Override
    public int getSize() { return size; }
}

// Composite
class Directory extends FileSystemComponent {
    private List<FileSystemComponent> children = new ArrayList<>();
    
    public Directory(String name) {
        this.name = name;
    }
    
    @Override
    public void add(FileSystemComponent component) {
        children.add(component);
    }
    
    @Override
    public void display(int depth) {
        System.out.println("-".repeat(depth) + name + "/");
        for(FileSystemComponent component : children) {
            component.display(depth + 2);
        }
    }
    
    @Override
    public int getSize() {
        return children.stream().mapToInt(FileSystemComponent::getSize).sum();
    }
}
```

---

### 2.3 Proxy Pattern

**Theory:**
The Proxy pattern provides a surrogate or placeholder for another object to control access to it. It creates a representative object that controls access to the original object, allowing you to perform additional operations before or after the request reaches the original object.

**UML Diagram:**
```
    Client  ────────►  Subject
                         ↑
                    ┌────┴────┐
                    │         │
              RealSubject   Proxy ──────► RealSubject
```

**Types:**
- **Virtual Proxy**: Controls access to expensive resources
- **Protection Proxy**: Controls access based on permissions
- **Remote Proxy**: Provides local representative for remote objects

**Partial Code Example:**
```java
// Subject interface
interface Image {
    void display();
}

// RealSubject
class RealImage implements Image {
    private String filename;
    
    public RealImage(String filename) {
        this.filename = filename;
        loadFromDisk();
    }
    
    private void loadFromDisk() {
        System.out.println("Loading " + filename);
        // Expensive operation
    }
    
    @Override
    public void display() {
        System.out.println("Displaying " + filename);
    }
}

// Proxy
class ProxyImage implements Image {
    private RealImage realImage;
    private String filename;
    
    public ProxyImage(String filename) {
        this.filename = filename;
    }
    
    @Override
    public void display() {
        if(realImage == null) {
            realImage = new RealImage(filename); // Lazy loading
        }
        realImage.display();
    }
}
```

---

### 2.4 Flyweight Pattern

**Theory:**
The Flyweight pattern minimizes memory usage by sharing efficiently common data among multiple objects. It separates intrinsic state (shared) from extrinsic state (context-dependent), storing intrinsic state in flyweight objects and passing extrinsic state as parameters.

**UML Diagram:**
```
    Client ──────► Context
      │              │
      │              ▼
      └──────► Flyweight ◄──── FlyweightFactory
                   ↑                    │
                   │                    │
              ConcreteFlyweight ◄───────┘
```

**Structure:**
- **Flyweight**: Interface for flyweights to receive extrinsic state
- **ConcreteFlyweight**: Implements flyweight interface and stores intrinsic state
- **FlyweightFactory**: Creates and manages flyweight objects
- **Context**: Contains extrinsic state

**Partial Code Example:**
```java
// Flyweight interface
interface Shape {
    void draw(int x, int y, int radius); // extrinsic state as parameters
}

// Concrete Flyweight
class Circle implements Shape {
    private String color; // intrinsic state
    
    public Circle(String color) {
        this.color = color;
    }
    
    @Override
    public void draw(int x, int y, int radius) {
        System.out.println("Drawing " + color + " circle at (" + x + "," + y + ") radius=" + radius);
    }
}

// Flyweight Factory
class ShapeFactory {
    private static final Map<String, Shape> circleMap = new HashMap<>();
    
    public static Shape getCircle(String color) {
        Circle circle = (Circle) circleMap.get(color);
        
        if(circle == null) {
            circle = new Circle(color);
            circleMap.put(color, circle);
            System.out.println("Creating circle of color: " + color);
        }
        return circle;
    }
    
    public static int getCreatedShapes() {
        return circleMap.size();
    }
}
```

---

### 2.5 Facade Pattern

**Theory:**
The Facade pattern provides a simplified interface to a complex subsystem. It defines a higher-level interface that makes the subsystem easier to use by hiding the complexities of the subsystem from clients. This pattern promotes loose coupling between clients and subsystems.

**UML Diagram:**
```
    Client ──────► Facade ──────► Subsystem Classes
                     │              ├─ ClassA
                     │              ├─ ClassB
                     │              ├─ ClassC
                     └──────────────└─ ClassD
```

**Structure:**
- **Facade**: Provides simple methods that delegate to subsystem classes
- **Subsystem Classes**: Complex classes that perform the actual work
- **Client**: Uses the Facade instead of calling subsystem classes directly

**Partial Code Example:**
```java
// Subsystem classes
class DVDPlayer {
    public void on() { System.out.println("DVD Player on"); }
    public void play(String movie) { System.out.println("Playing: " + movie); }
    public void off() { System.out.println("DVD Player off"); }
}

class Amplifier {
    public void on() { System.out.println("Amplifier on"); }
    public void setVolume(int level) { System.out.println("Volume set to " + level); }
    public void off() { System.out.println("Amplifier off"); }
}

class Projector {
    public void on() { System.out.println("Projector on"); }
    public void wideScreenMode() { System.out.println("Projector wide screen"); }
    public void off() { System.out.println("Projector off"); }
}

// Facade
class HomeTheaterFacade {
    private DVDPlayer dvdPlayer;
    private Amplifier amplifier;
    private Projector projector;
    
    public HomeTheaterFacade(DVDPlayer dvd, Amplifier amp, Projector proj) {
        this.dvdPlayer = dvd;
        this.amplifier = amp;
        this.projector = proj;
    }
    
    public void watchMovie(String movie) {
        System.out.println("Get ready to watch a movie...");
        projector.on();
        projector.wideScreenMode();
        amplifier.on();
        amplifier.setVolume(5);
        dvdPlayer.on();
        dvdPlayer.play(movie);
    }
    
    public void endMovie() {
        System.out.println("Shutting movie theater down...");
        // ... shutdown sequence
    }
}
```

---

### 2.6 Bridge Pattern

**Theory:**
The Bridge pattern separates an abstraction from its implementation, allowing both to vary independently. It uses composition instead of inheritance to connect the abstraction and implementation hierarchies. This pattern is useful when you want to avoid permanent binding between an abstraction and its implementation.

**UML Diagram:**
```
    Abstraction ──────► Implementation
         ↑                     ↑
         │                     │
  RefinedAbstraction    ConcreteImplementation
```

**Structure:**
- **Abstraction**: Defines the abstraction's interface and maintains reference to implementor
- **RefinedAbstraction**: Extends the abstraction
- **Implementation**: Defines interface for implementation classes
- **ConcreteImplementation**: Implements the Implementation interface

**Partial Code Example:**
```java
// Implementation interface
interface DrawingAPI {
    void drawCircle(double x, double y, double radius);
}

// Concrete Implementations
class DrawingAPI1 implements DrawingAPI {
    @Override
    public void drawCircle(double x, double y, double radius) {
        System.out.printf("API1.circle at %f:%f radius %f%n", x, y, radius);
    }
}

class DrawingAPI2 implements DrawingAPI {
    @Override
    public void drawCircle(double x, double y, double radius) {
        System.out.printf("API2.circle at %f:%f radius %f%n", x, y, radius);
    }
}

// Abstraction
abstract class Shape {
    protected DrawingAPI drawingAPI;
    
    protected Shape(DrawingAPI drawingAPI) {
        this.drawingAPI = drawingAPI;
    }
    
    public abstract void draw();
    public abstract void resizeByPercentage(double pct);
}

// Refined Abstraction
class CircleShape extends Shape {
    private double x, y, radius;
    
    public CircleShape(double x, double y, double radius, DrawingAPI drawingAPI) {
        super(drawingAPI);
        this.x = x; this.y = y; this.radius = radius;
    }
    
    @Override
    public void draw() {
        drawingAPI.drawCircle(x, y, radius);
    }
    
    @Override
    public void resizeByPercentage(double pct) {
        radius *= (1.0 + pct/100.0);
    }
}
```

---

### 2.7 Decorator Pattern

**Theory:**
The Decorator pattern allows behavior to be added to objects dynamically without altering their structure. It provides a flexible alternative to subclassing for extending functionality by wrapping objects in decorator classes that contain the additional behavior.

**UML Diagram:**
```
      Component
         ↑
    ┌────┴────┐
    │         │
ConcreteComponent  Decorator ──────► Component
              │         ↑
              │         │
              └── ConcreteDecorator
```

**Structure:**
- **Component**: Common interface for objects that can be decorated
- **ConcreteComponent**: Basic implementation of Component
- **Decorator**: Abstract decorator class that wraps a Component
- **ConcreteDecorator**: Adds specific functionality to the Component

**Partial Code Example:**
```java
// Component interface
interface Coffee {
    String getDescription();
    double getCost();
}

// Concrete Component
class SimpleCoffee implements Coffee {
    @Override
    public String getDescription() {
        return "Simple Coffee";
    }
    
    @Override
    public double getCost() {
        return 2.0;
    }
}

// Base Decorator
abstract class CoffeeDecorator implements Coffee {
    protected Coffee coffee;
    
    public CoffeeDecorator(Coffee coffee) {
        this.coffee = coffee;
    }
    
    @Override
    public String getDescription() {
        return coffee.getDescription();
    }
    
    @Override
    public double getCost() {
        return coffee.getCost();
    }
}

// Concrete Decorators
class MilkDecorator extends CoffeeDecorator {
    public MilkDecorator(Coffee coffee) {
        super(coffee);
    }
    
    @Override
    public String getDescription() {
        return coffee.getDescription() + ", Milk";
    }
    
    @Override
    public double getCost() {
        return coffee.getCost() + 0.5;
    }
}

class SugarDecorator extends CoffeeDecorator {
    public SugarDecorator(Coffee coffee) {
        super(coffee);
    }
    
    @Override
    public String getDescription() {
        return coffee.getDescription() + ", Sugar";
    }
    
    @Override
    public double getCost() {
        return coffee.getCost() + 0.2;
    }
}
```

---

## 3. Behavioral Design Patterns

### 3.1 Template Method Pattern

**Theory:**
The Template Method pattern defines the skeleton of an algorithm in a base class, letting subclasses override specific steps without changing the algorithm's structure. It promotes code reuse and ensures that the overall algorithm structure remains consistent across different implementations.

**UML Diagram:**
```
    AbstractClass
    ┌─ templateMethod()
    ├─ primitiveOperation1()  [abstract]
    └─ primitiveOperation2()  [abstract]
         ↑
         │
    ConcreteClass
    ├─ primitiveOperation1()  [implemented]
    └─ primitiveOperation2()  [implemented]
```

**Structure:**
- **AbstractClass**: Defines template method and abstract primitive operations
- **ConcreteClass**: Implements primitive operations for specific algorithm steps

**Partial Code Example:**
```java
// Abstract class defining template method
abstract class DataProcessor {
    
    // Template method - defines algorithm skeleton
    public final void processData() {
        readData();
        processDataImpl();
        writeData();
    }
    
    // Concrete methods
    private void readData() {
        System.out.println("Reading data from source");
    }
    
    private void writeData() {
        System.out.println("Writing processed data");
    }
    
    // Abstract method to be implemented by subclasses
    protected abstract void processDataImpl();
}

// Concrete implementations
class CSVDataProcessor extends DataProcessor {
    @Override
    protected void processDataImpl() {
        System.out.println("Processing CSV data");
        // CSV-specific processing logic
    }
}

class XMLDataProcessor extends DataProcessor {
    @Override
    protected void processDataImpl() {
        System.out.println("Processing XML data");
        // XML-specific processing logic
    }
}
```

---

### 3.2 Mediator Pattern

**Theory:**
The Mediator pattern defines how a set of objects interact with each other. Instead of objects communicating directly, they communicate through a central mediator object. This pattern promotes loose coupling by keeping objects from referring to each other explicitly.

**UML Diagram:**
```
    Mediator ◄──────── ConcreteMediator ──────► Colleague
       ↑                      │                    ↑
       │                      │               ┌────┴────┐
       │                      │               │         │
       └──────────────────────┘        ConcreteColleague1  ConcreteColleague2
```

**Structure:**
- **Mediator**: Interface defining communication contract
- **ConcreteMediator**: Implements mediator and coordinates communication
- **Colleague**: Abstract base for objects that communicate through mediator
- **ConcreteColleague**: Specific objects that interact via mediator

**Partial Code Example:**
```java
// Mediator interface
interface ChatMediator {
    void sendMessage(String message, User user);
    void addUser(User user);
}

// Concrete Mediator
class ConcreteChatMediator implements ChatMediator {
    private List<User> users = new ArrayList<>();
    
    @Override
    public void addUser(User user) {
        users.add(user);
    }
    
    @Override
    public void sendMessage(String message, User sender) {
        for(User user : users) {
            if(user != sender) {
                user.receive(message, sender.getName());
            }
        }
    }
}

// Colleague
abstract class User {
    protected ChatMediator mediator;
    protected String name;
    
    public User(ChatMediator mediator, String name) {
        this.mediator = mediator;
        this.name = name;
    }
    
    public abstract void send(String message);
    public abstract void receive(String message, String from);
    
    public String getName() { return name; }
}

// Concrete Colleague
class ConcreteUser extends User {
    public ConcreteUser(ChatMediator mediator, String name) {
        super(mediator, name);
    }
    
    @Override
    public void send(String message) {
        System.out.println(name + " sending: " + message);
        mediator.sendMessage(message, this);
    }
    
    @Override
    public void receive(String message, String from) {
        System.out.println(name + " received: " + message + " from " + from);
    }
}
```

---

### 3.3 Chain of Responsibility Pattern

**Theory:**
The Chain of Responsibility pattern passes requests along a chain of handlers. Each handler decides either to process the request or pass it to the next handler in the chain. This pattern decouples senders and receivers of requests and allows multiple objects to handle the request without the sender knowing which object will handle it.

**UML Diagram:**
```
    Client ──────► Handler ◄─────┐
                      ↑         │
                      │         │
               ConcreteHandler ──┘
                      │
               nextHandler
```

**Structure:**
- **Handler**: Abstract base class defining interface and chain link
- **ConcreteHandler**: Handles requests it's responsible for, passes others to successor
- **Client**: Initiates request to chain

**Partial Code Example:**
```java
// Handler abstract class
abstract class SupportHandler {
    protected SupportHandler nextHandler;
    
    public void setNextHandler(SupportHandler nextHandler) {
        this.nextHandler = nextHandler;
    }
    
    public abstract void handleRequest(SupportRequest request);
}

// Support request class
class SupportRequest {
    private RequestType type;
    private String description;
    
    public SupportRequest(RequestType type, String description) {
        this.type = type;
        this.description = description;
    }
    
    // getters...
    public RequestType getType() { return type; }
    public String getDescription() { return description; }
}

enum RequestType { BASIC, INTERMEDIATE, CRITICAL }

// Concrete Handlers
class Level1Support extends SupportHandler {
    @Override
    public void handleRequest(SupportRequest request) {
        if(request.getType() == RequestType.BASIC) {
            System.out.println("Level 1 Support handled: " + request.getDescription());
        } else if(nextHandler != null) {
            nextHandler.handleRequest(request);
        }
    }
}

class Level2Support extends SupportHandler {
    @Override
    public void handleRequest(SupportRequest request) {
        if(request.getType() == RequestType.INTERMEDIATE) {
            System.out.println("Level 2 Support handled: " + request.getDescription());
        } else if(nextHandler != null) {
            nextHandler.handleRequest(request);
        }
    }
}

class Level3Support extends SupportHandler {
    @Override
    public void handleRequest(SupportRequest request) {
        if(request.getType() == RequestType.CRITICAL) {
            System.out.println("Level 3 Support handled: " + request.getDescription());
        } else {
            System.out.println("Request cannot be handled");
        }
    }
}
```

---

### 3.4 Observer Pattern

**Theory:**
The Observer pattern defines a one-to-many dependency between objects so that when one object changes state, all its dependents are notified automatically. This pattern is fundamental to implementing distributed event handling systems and the Model-View architecture.

**UML Diagram:**
```
    Subject ◄──────── ConcreteSubject
       ↑                    │
       │                    │ notify()
    Observer ◄───────── ConcreteObserver
       ↑
       │
   update()
```

**Structure:**
- **Subject**: Maintains list of observers and provides methods to add/remove observers
- **Observer**: Interface for objects that should be notified of changes
- **ConcreteSubject**: Stores state and notifies observers when state changes
- **ConcreteObserver**: Implements Observer interface to receive notifications

**Partial Code Example:**
```java
// Observer interface
interface Observer {
    void update(String news);
}

// Subject interface
interface Subject {
    void addObserver(Observer observer);
    void removeObserver(Observer observer);
    void notifyObservers();
}

// Concrete Subject
class NewsAgency implements Subject {
    private List<Observer> observers = new ArrayList<>();
    private String news;
    
    @Override
    public void addObserver(Observer observer) {
        observers.add(observer);
    }
    
    @Override
    public void removeObserver(Observer observer) {
        observers.remove(observer);
    }
    
    @Override
    public void notifyObservers() {
        for(Observer observer : observers) {
            observer.update(news);
        }
    }
    
    public void setNews(String news) {
        this.news = news;
        notifyObservers();
    }
}

// Concrete Observer
class NewsChannel implements Observer {
    private String channelName;
    private String latestNews;
    
    public NewsChannel(String channelName) {
        this.channelName = channelName;
    }
    
    @Override
    public void update(String news) {
        this.latestNews = news;
        System.out.println(channelName + " received news: " + news);
    }
}
```

---

### 3.5 Strategy Pattern

**Theory:**
The Strategy pattern defines a family of algorithms, encapsulates each one, and makes them interchangeable. It lets the algorithm vary independently from clients that use it. This pattern is useful when you have multiple ways to perform a task and want to choose the algorithm at runtime.

**UML Diagram:**
```
    Context ──────► Strategy
                       ↑
                  ┌────┴────┐
                  │         │
           ConcreteStrategyA  ConcreteStrategyB
```

**Structure:**
- **Context**: Maintains reference to Strategy object and delegates algorithm execution
- **Strategy**: Common interface for all concrete strategies
- **ConcreteStrategy**: Implements specific algorithm using Strategy interface

**Partial Code Example:**
```java
// Strategy interface
interface PaymentStrategy {
    void pay(double amount);
}

// Concrete Strategies
class CreditCardPayment implements PaymentStrategy {
    private String cardNumber;
    private String name;
    
    public CreditCardPayment(String cardNumber, String name) {
        this.cardNumber = cardNumber;
        this.name = name;
    }
    
    @Override
    public void pay(double amount) {
        System.out.println("Paid $" + amount + " using Credit Card ending in " + 
                          cardNumber.substring(cardNumber.length()-4));
    }
}

class PayPalPayment implements PaymentStrategy {
    private String email;
    
    public PayPalPayment(String email) {
        this.email = email;
    }
    
    @Override
    public void pay(double amount) {
        System.out.println("Paid $" + amount + " using PayPal account: " + email);
    }
}

// Context
class ShoppingCart {
    private List<Item> items = new ArrayList<>();
    private PaymentStrategy paymentStrategy;
    
    public void addItem(Item item) {
        items.add(item);
    }
    
    public void setPaymentStrategy(PaymentStrategy paymentStrategy) {
        this.paymentStrategy = paymentStrategy;
    }
    
    public void checkout() {
        double total = items.stream().mapToDouble(Item::getPrice).sum();
        paymentStrategy.pay(total);
    }
}

class Item {
    private String name;
    private double price;
    
    // constructor and getters...
    public double getPrice() { return price; }
}
```

---

### 3.6 Command Pattern

**Theory:**
The Command pattern encapsulates a request as an object, allowing you to parameterize clients with different requests, queue operations, and support undo operations. It decouples the object that invokes the operation from the object that performs it.

**UML Diagram:**
```
    Client ──────► Command ◄───── ConcreteCommand ──────► Receiver
                      ↑                                      │
                      │                                   action()
                   Invoker
```

**Structure:**
- **Command**: Interface for executing operations
- **ConcreteCommand**: Implements Command and defines binding between Receiver and action
- **Receiver**: Knows how to perform operations associated with request
- **Invoker**: Asks command to carry out the request
- **Client**: Creates ConcreteCommand and sets its receiver

**Partial Code Example:**
```java
// Command interface
interface Command {
    void execute();
    void undo();
}

// Receiver
class Light {
    private boolean isOn = false;
    
    public void turnOn() {
        isOn = true;
        System.out.println("Light is ON");
    }
    
    public void turnOff() {
        isOn = false;
        System.out.println("Light is OFF");
    }
}

// Concrete Commands
class LightOnCommand implements Command {
    private Light light;
    
    public LightOnCommand(Light light) {
        this.light = light;
    }
    
    @Override
    public void execute() {
        light.turnOn();
    }
    
    @Override
    public void undo() {
        light.turnOff();
    }
}

class LightOffCommand implements Command {
    private Light light;
    
    public LightOffCommand(Light light) {
        this.light = light;
    }
    
    @Override
    public void execute() {
        light.turnOff();
    }
    
    @Override
    public void undo() {
        light.turnOn();
    }
}

// Invoker
class RemoteControl {
    private Command[] onCommands;
    private Command[] offCommands;
    private Command undoCommand;
    
    public RemoteControl() {
        onCommands = new Command[7];
        offCommands = new Command[7];
        // Initialize with NoCommand objects...
    }
    
    public void setCommand(int slot, Command onCommand, Command offCommand) {
        onCommands[slot] = onCommand;
        offCommands[slot] = offCommand;
    }
    
    public void onButtonPressed(int slot) {
        onCommands[slot].execute();
        undoCommand = onCommands[slot];
    }
    
    public void undoButtonPressed() {
        undoCommand.undo();
    }
}
```

---

### 3.7 State Pattern

**Theory:**
The State pattern allows an object to alter its behavior when its internal state changes. The object appears to change its class. This pattern is useful for implementing state machines and eliminates the need for large conditional statements.

**UML Diagram:**
```
    Context ──────► State
       │               ↑
       │          ┌────┴────┐
    state          │         │
                ConcreteStateA  ConcreteStateB
```

**Structure:**
- **Context**: Maintains instance of ConcreteState and delegates state-specific behavior
- **State**: Interface for encapsulating behavior associated with state
- **ConcreteState**: Implements behavior associated with specific state

**Partial Code Example:**
```java
// State interface
interface State {
    void insertQuarter(GumballMachine machine);
    void ejectQuarter(GumballMachine machine);
    void turnCrank(GumballMachine machine);
    void dispense(GumballMachine machine);
}

// Context
class GumballMachine {
    private State soldOutState;
    private State noQuarterState;
    private State hasQuarterState;
    private State soldState;
    
    private State currentState;
    private int count = 0;
    
    public GumballMachine(int numberGumballs) {
        soldOutState = new SoldOutState();
        noQuarterState = new NoQuarterState();
        hasQuarterState = new HasQuarterState();
        soldState = new SoldState();
        
        this.count = numberGumballs;
        if (numberGumballs > 0) {
            currentState = noQuarterState;
        } else {
            currentState = soldOutState;
        }
    }
    
    public void insertQuarter() { currentState.insertQuarter(this); }
    public void ejectQuarter() { currentState.ejectQuarter(this); }
    public void turnCrank() { 
        currentState.turnCrank(this);
        currentState.dispense(this);
    }
    
    public void setState(State state) { this.currentState = state; }
    public void releaseBall() {
        System.out.println("A gumball comes rolling out the slot...");
        if (count != 0) count--;
    }
    
    // Getters for states...
    public State getSoldOutState() { return soldOutState; }
    public State getNoQuarterState() { return noQuarterState; }
    public State getHasQuarterState() { return hasQuarterState; }
    public State getSoldState() { return soldState; }
    public int getCount() { return count; }
}

// Concrete States
class NoQuarterState implements State {
    @Override
    public void insertQuarter(GumballMachine machine) {
        System.out.println("You inserted a quarter");
        machine.setState(machine.getHasQuarterState());
    }
    
    @Override
    public void ejectQuarter(GumballMachine machine) {
        System.out.println("You haven't inserted a quarter");
    }
    
    @Override
    public void turnCrank(GumballMachine machine) {
        System.out.println("You turned, but there's no quarter");
    }
    
    @Override
    public void dispense(GumballMachine machine) {
        System.out.println("You need to pay first");
    }
}

class HasQuarterState implements State {
    @Override
    public void insertQuarter(GumballMachine machine) {
        System.out.println("You can't insert another quarter");
    }
    
    @Override
    public void ejectQuarter(GumballMachine machine) {
        System.out.println("Quarter returned");
        machine.setState(machine.getNoQuarterState());
    }
    
    @Override
    public void turnCrank(GumballMachine machine) {
        System.out.println("You turned...");
        machine.setState(machine.getSoldState());
    }
    
    @Override
    public void dispense(GumballMachine machine) {
        System.out.println("No gumball dispensed");
    }
}



//End of the assignment..