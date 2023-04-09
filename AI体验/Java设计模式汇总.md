

# Java设计模式汇总

## 1. 创建型模式

### 1.1 工厂模式

简单工厂模式

- 简单工厂模式示例：

```
interface Product {
    void use();
}

class ConcreteProduct1 implements Product {
    @Override
    public void use() {
        System.out.println("使用产品1");
    }
}

class ConcreteProduct2 implements Product {
    @Override
    public void use() {
        System.out.println("使用产品2");
    }
}

class SimpleFactory {
    public static Product createProduct(String type) {
        if ("1".equals(type)) {
            return new ConcreteProduct1();
        } else if ("2".equals(type)) {
            return new ConcreteProduct2();
        } else {
            throw new IllegalArgumentException("不支持的产品类型：" + type);
        }
    }
}

public class Client {
    public static void main(String[] args) {
        Product product1 = SimpleFactory.createProduct("1");
        product1.use();
        Product product2 = SimpleFactory.createProduct("2");
        product2.use();
    }
}
```

输出结果：

```
使用产品1
使用产品2
```

工厂方法模式

- 工厂方法模式是一种创建型设计模式，它提供了一种创建对象的最佳方式。
- 工厂方法模式使用工厂方法来处理对象的创建，而不是在代码中直接使用 new 操作符。
- 工厂方法模式可以让我们在不改变现有代码的情况下向应用程序中添加新的产品。
- 工厂方法模式包含四个角色：抽象产品、具体产品、抽象工厂和具体工厂。
- 抽象产品是被创建的对象的抽象接口，具体产品是抽象产品的具体实现。
- 抽象工厂是创建抽象产品的工厂接口，具体工厂是抽象工厂的具体实现。
- 工厂方法模式的优点包括：封装了对象的创建过程、使得代码更容易维护、对扩展开放、对修改关闭。
- 工厂方法模式的缺点包括：增加了系统的抽象性和理解难度、增加了系统的复杂度和代码量。

抽象工厂模式

### 1.2 单例模式

- 抽象工厂模式

抽象工厂模式是一种创建型设计模式，它允许客户端创建一组相关的对象，而无需指定其具体类。抽象工厂模式定义了一个接口，用于创建相关对象的工厂，而不必指定其具体类。通过使用该接口，客户端可以创建多个对象，而无需了解这些对象的实现细节。

- 单例模式

单例模式是一种创建型设计模式，它保证一个类只有一个实例，并提供一个全局访问点。单例模式通常用于控制资源的访问，例如数据库连接或线程池。在单例模式中，类的构造函数必须是私有的，以便防止客户端直接创建实例。类还必须提供一个静态方法，以便客户端可以访问类的唯一实例。

饿汉式单例模式

- 饿汉式单例模式

```
public class Singleton {
    private static Singleton instance = new Singleton();
    private Singleton() {}
    public static Singleton getInstance() {
        return instance;
    }
}
```

懒汉式单例模式

- 懒汉式单例模式
  
  ```
  public class LazySingleton {
      private static LazySingleton instance;
  
      private LazySingleton() {}
  
      public static synchronized LazySingleton getInstance() {
          if (instance == null) {
              instance = new LazySingleton();
          }
          return instance;
      }
  }
  ```

双重检查锁单例模式

- 双重检查锁单例模式
  
  ```
  public class Singleton {
      private volatile static Singleton uniqueInstance;
      private Singleton() {}
      public static Singleton getInstance() {
          if (uniqueInstance == null) {
              synchronized (Singleton.class) {
                  if (uniqueInstance == null) {
                      uniqueInstance = new Singleton();
                  }
              }
          }
          return uniqueInstance;
      }
  }
  ```

静态内部类单例模式

### 1.3 建造者模式

- 静态内部类单例模式
  
  ```
  public class Singleton {
      private Singleton() {}
  
      private static class SingletonHolder {
          private static final Singleton INSTANCE = new Singleton();
      }
  
      public static Singleton getInstance() {
          return SingletonHolder.INSTANCE;
      }
  }
  ```

- 建造者模式
  
  ```
  public class Product {
      private String partA;
      private String partB;
      private String partC;
  
      public void setPartA(String partA) {
          this.partA = partA;
      }
  
      public void setPartB(String partB) {
          this.partB = partB;
      }
  
      public void setPartC(String partC) {
          this.partC = partC;
      }
  }
  
  public interface Builder {
      void buildPartA();
      void buildPartB();
      void buildPartC();
      Product getResult();
  }
  
  public class ConcreteBuilder implements Builder {
      private Product product = new Product();
  
      public void buildPartA() {
          product.setPartA("PartA");
      }
  
      public void buildPartB() {
          product.setPartB("PartB");
      }
  
      public void buildPartC() {
          product.setPartC("PartC");
      }
  
      public Product getResult() {
          return product;
      }
  }
  
  public class Director {
      public void construct(Builder builder) {
          builder.buildPartA();
          builder.buildPartB();
          builder.buildPartC();
      }
  }
  
  public class Client {
      public static void main(String[] args) {
          Builder builder = new ConcreteBuilder();
          Director director = new Director();
          director.construct(builder);
          Product product = builder.getResult();
      }
  }
  ```

普通建造者模式

- 普通建造者模式
  
  ```
  public class Product {
      private String partA;
      private String partB;
      private String partC;
  
      public String getPartA() {
          return partA;
      }
  
      public void setPartA(String partA) {
          this.partA = partA;
      }
  
      public String getPartB() {
          return partB;
      }
  
      public void setPartB(String partB) {
          this.partB = partB;
      }
  
      public String getPartC() {
          return partC;
      }
  
      public void setPartC(String partC) {
          this.partC = partC;
      }
  }
  
  public abstract class Builder {
      protected Product product = new Product();
  
      public abstract void buildPartA();
  
      public abstract void buildPartB();
  
      public abstract void buildPartC();
  
      public Product getResult() {
          return product;
      }
  }
  
  public class ConcreteBuilder extends Builder {
      @Override
      public void buildPartA() {
          product.setPartA("PartA");
      }
  
      @Override
      public void buildPartB() {
          product.setPartB("PartB");
      }
  
      @Override
      public void buildPartC() {
          product.setPartC("PartC");
      }
  }
  
  public class Director {
      private Builder builder;
  
      public Director(Builder builder) {
          this.builder = builder;
      }
  
      public void construct() {
          builder.buildPartA();
          builder.buildPartB();
          builder.buildPartC();
      }
  }
  
  public class Client {
      public static void main(String[] args) {
          Builder builder = new ConcreteBuilder();
          Director director = new Director(builder);
          director.construct();
          Product product = builder.getResult();
          System.out.println(product.getPartA());
          System.out.println(product.getPartB());
          System.out.println(product.getPartC());
      }
  }
  ```

可配置的建造者模式

### 1.1 工厂模式

- 可配置的建造者模式
  
  ```
  public interface Builder {
      void buildPartA();
      void buildPartB();
      void buildPartC();
      Product getResult();
  }
  
  public class ConcreteBuilder implements Builder {
      private Product product = new Product();
  
      @Override
      public void buildPartA() {
          product.setPartA("PartA");
      }
  
      @Override
      public void buildPartB() {
          product.setPartB("PartB");
      }
  
      @Override
      public void buildPartC() {
          product.setPartC("PartC");
      }
  
      @Override
      public Product getResult() {
          return product;
      }
  }
  
  public class Director {
      private Builder builder;
  
      public Director(Builder builder) {
          this.builder = builder;
      }
  
      public void construct() {
          builder.buildPartA();
          builder.buildPartB();
          builder.buildPartC();
      }
  }
  
  public class Product {
      private String partA;
      private String partB;
      private String partC;
  
      public void setPartA(String partA) {
          this.partA = partA;
      }
  
      public void setPartB(String partB) {
          this.partB = partB;
      }
  
      public void setPartC(String partC) {
          this.partC = partC;
      }
  
      @Override
      public String toString() {
          return "Product{" +
                  "partA='" + partA + '\'' +
                  ", partB='" + partB + '\'' +
                  ", partC='" + partC + '\'' +
                  '}';
      }
  }
  
  public class Client {
      public static void main(String[] args) {
          Builder builder = new ConcreteBuilder();
          Director director = new Director(builder);
          director.construct();
          Product product = builder.getResult();
          System.out.println(product);
      }
  }
  ```

## 2. 结构型模式

### 2.1 适配器模式

类适配器模式

- 类适配器模式实例：

假设我们有一个已经存在的类，其中有一个方法可以输出一个字符串，但我们需要将其改为输出整数。这时候我们可以使用适配器模式来实现：

```
// 已经存在的类，其中有一个输出字符串的方法
class Adaptee {
    public String outputString() {
        return "42";
    }
}

// 目标接口，需要输出整数的方法
interface Target {
    public int outputInt();
}

// 类适配器，继承已经存在的类并实现目标接口
class ClassAdapter extends Adaptee implements Target {
    public int outputInt() {
        String str = outputString();
        return Integer.parseInt(str);
    }
}

// 测试类
public class Test {
    public static void main(String[] args) {
        Target target = new ClassAdapter();
        int result = target.outputInt();
        System.out.println(result); // 输出：42
    }
}
```

在这个例子中，我们已经有了一个已经存在的类Adaptee，其中有一个方法outputString()可以输出一个字符串。但是我们需要将其改为输出整数。这时候我们可以使用适配器模式来实现。

我们定义一个目标接口Target，其中有一个方法outputInt()可以输出整数。然后我们定义一个类ClassAdapter，它继承了已经存在的类Adaptee，并实现了目标接口Target。在ClassAdapter中，我们重写了目标接口中的方法outputInt()，在其中调用了已经存在的类Adaptee中的方法outputString()，并将其转换为整数输出。最后我们在测试类中创建了ClassAdapter的实例，并调用了目标接口中的方法outputInt()，得到了输出结果42。

对象适配器模式

- 对象适配器模式
  
  ```
  // 目标接口
  interface Target {
      void request();
  }
  
  // 适配者类
  class Adaptee {
      public void specificRequest() {
          System.out.println("适配者中的业务代码被调用！");
      }
  }
  
  // 对象适配器类
  class ObjectAdapter implements Target {
      private Adaptee adaptee;
  
      public ObjectAdapter(Adaptee adaptee) {
          this.adaptee = adaptee;
      }
  
      @Override
      public void request() {
          adaptee.specificRequest();
      }
  }
  
  // 测试类
  public class Client {
      public static void main(String[] args) {
          Adaptee adaptee = new Adaptee();
          Target target = new ObjectAdapter(adaptee);
          target.request();
      }
  }
  ```

接口适配器模式

### 2.2 装饰器模式

- 接口适配器模式
  
  ```
  public interface Target {
      void request();
  }
  
  public class Adaptee {
      public void specificRequest() {
          System.out.println("Adaptee specificRequest");
      }
  }
  
  public class Adapter extends Adaptee implements Target {
      @Override
      public void request() {
          specificRequest();
      }
  }
  
  public class Client {
      public static void main(String[] args) {
          Target target = new Adapter();
          target.request();
      }
  }
  ```

- 装饰器模式
  
  ```
  public interface Component {
      void operation();
  }
  
  public class ConcreteComponent implements Component {
      @Override
      public void operation() {
          System.out.println("ConcreteComponent operation");
      }
  }
  
  public abstract class Decorator implements Component {
      protected Component component;
  
      public Decorator(Component component) {
          this.component = component;
      }
  
      @Override
      public void operation() {
          component.operation();
      }
  }
  
  public class ConcreteDecoratorA extends Decorator {
      public ConcreteDecoratorA(Component component) {
          super(component);
      }
  
      @Override
      public void operation() {
          super.operation();
          System.out.println("ConcreteDecoratorA operation");
      }
  }
  
  public class ConcreteDecoratorB extends Decorator {
      public ConcreteDecoratorB(Component component) {
          super(component);
      }
  
      @Override
      public void operation() {
          super.operation();
          System.out.println("ConcreteDecoratorB operation");
      }
  }
  
  public class Client {
      public static void main(String[] args) {
          Component component = new ConcreteComponent();
          component = new ConcreteDecoratorA(component);
          component = new ConcreteDecoratorB(component);
          component.operation();
      }
  }
  ```

装饰器模式

### 2.3 代理模式

- 装饰器模式
  
  装饰器模式是一种结构型设计模式，它允许你通过将对象放入包含行为的特殊对象中来为原对象绑定新的行为。
  
  ```
  public interface Shape {
      void draw();
  }
  
  public class Circle implements Shape {
      @Override
      public void draw() {
          System.out.println("Shape: Circle");
      }
  }
  
  public abstract class ShapeDecorator implements Shape {
      protected Shape decoratedShape;
  
      public ShapeDecorator(Shape decoratedShape){
          this.decoratedShape = decoratedShape;
      }
  
      public void draw(){
          decoratedShape.draw();
      }
  }
  
  public class RedShapeDecorator extends ShapeDecorator {
  
      public RedShapeDecorator(Shape decoratedShape) {
          super(decoratedShape);
      }
  
      @Override
      public void draw() {
          decoratedShape.draw();
          setRedBorder(decoratedShape);
      }
  
      private void setRedBorder(Shape decoratedShape){
          System.out.println("Border Color: Red");
      }
  }
  
  public class DecoratorPatternDemo {
      public static void main(String[] args) {
  
          Shape circle = new Circle();
  
          Shape redCircle = new RedShapeDecorator(new Circle());
  
          Shape redRectangle = new RedShapeDecorator(new Rectangle());
          System.out.println("Circle with normal border");
          circle.draw();
  
          System.out.println("\nCircle of red border");
          redCircle.draw();
  
          System.out.println("\nRectangle of red border");
          redRectangle.draw();
      }
  }
  ```

- 代理模式
  
  代理模式是一种结构型设计模式，让你能够提供对象的替代品或其占位符。代理控制着对于原对象的访问，并允许在将请求提交给对象前后进行一些处理。
  
  ```
  public interface Image {
      void display();
  }
  
  public class RealImage implements Image {
  
      private String fileName;
  
      public RealImage(String fileName){
          this.fileName = fileName;
          loadFromDisk(fileName);
      }
  
      @Override
      public void display() {
          System.out.println("Displaying " + fileName);
      }
  
      private void loadFromDisk(String fileName){
          System.out.println("Loading " + fileName);
      }
  }
  
  public class ProxyImage implements Image{
  
      private RealImage realImage;
      private String fileName;
  
      public ProxyImage(String fileName){
          this.fileName = fileName;
      }
  
      @Override
      public void display() {
          if(realImage == null){
              realImage = new RealImage(fileName);
          }
          realImage.display();
      }
  }
  
  public class ProxyPatternDemo {
      public static void main(String[] args) {
          Image image = new ProxyImage("test_10mb.jpg");
  
          // 图像将从磁盘加载
          image.display();
          System.out.println("");
  
          // 图像不需要从磁盘加载
          image.display();
      }
  }
  ```

静态代理模式

- 静态代理模式实例：

假设我们有一个接口`Calculator`，里面有两个方法`add`和`subtract`，现在我们需要对这个接口进行代理，使得每次调用这两个方法时，都会输出一行日志。

首先，我们定义一个代理类`CalculatorProxy`，它实现了`Calculator`接口，并且持有一个`Calculator`类型的对象作为其成员变量。在代理类的方法中，我们先输出日志，然后再调用`Calculator`对象的对应方法。

```
public interface Calculator {
    int add(int a, int b);
    int subtract(int a, int b);
}

public class CalculatorImpl implements Calculator {
    @Override
    public int add(int a, int b) {
        return a + b;
    }

    @Override
    public int subtract(int a, int b) {
        return a - b;
    }
}

public class CalculatorProxy implements Calculator {
    private Calculator calculator;

    public CalculatorProxy(Calculator calculator) {
        this.calculator = calculator;
    }

    @Override
    public int add(int a, int b) {
        System.out.println("Before add operation");
        int result = calculator.add(a, b);
        System.out.println("After add operation");
        return result;
    }

    @Override
    public int subtract(int a, int b) {
        System.out.println("Before subtract operation");
        int result = calculator.subtract(a, b);
        System.out.println("After subtract operation");
        return result;
    }
}

public class Main {
    public static void main(String[] args) {
        Calculator calculator = new CalculatorImpl();
        Calculator proxy = new CalculatorProxy(calculator);
        int result1 = proxy.add(1, 2);
        int result2 = proxy.subtract(3, 1);
    }
}
```

动态代理模式

### 2.4 外观模式

- 动态代理模式
  
  动态代理模式是一种结构型设计模式，它使得在不改变原有类结构的情况下，通过代理类对目标对象进行间接访问和控制。动态代理模式分为两种：基于接口的动态代理和基于类的动态代理。
  
  示例：
  
  ```
  public interface Subject {
      void request();
  }
  
  public class RealSubject implements Subject {
      public void request() {
          System.out.println("RealSubject: Handling request.");
      }
  }
  
  public class DynamicProxy implements InvocationHandler {
      private Object subject;
  
      public DynamicProxy(Object subject) {
          this.subject = subject;
      }
  
      public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
          System.out.println("DynamicProxy: Handling request.");
          Object result = method.invoke(subject, args);
          return result;
      }
  }
  
  public class Client {
      public static void main(String[] args) {
          RealSubject realSubject = new RealSubject();
          InvocationHandler handler = new DynamicProxy(realSubject);
          Subject proxy = (Subject) Proxy.newProxyInstance(
                  realSubject.getClass().getClassLoader(),
                  realSubject.getClass().getInterfaces(),
                  handler
          );
          proxy.request();
      }
  }
  ```

- 外观模式
  
  外观模式是一种结构型设计模式，它为一组复杂的子系统接口提供一个更高层次的接口，使得子系统更易于使用。外观模式隐藏了子系统的复杂性，使得客户端可以像使用一个简单的接口一样使用子系统。
  
  示例：
  
  ```
  public class SubsystemA {
      public void operationA() {
          System.out.println("SubsystemA: Doing operation A.");
      }
  }
  
  public class SubsystemB {
      public void operationB() {
          System.out.println("SubsystemB: Doing operation B.");
      }
  }
  
  public class SubsystemC {
      public void operationC() {
          System.out.println("SubsystemC: Doing operation C.");
      }
  }
  
  public class Facade {
      private SubsystemA subsystemA;
      private SubsystemB subsystemB;
      private SubsystemC subsystemC;
  
      public Facade() {
          subsystemA = new SubsystemA();
          subsystemB = new SubsystemB();
          subsystemC = new SubsystemC();
      }
  
      public void operation() {
          subsystemA.operationA();
          subsystemB.operationB();
          subsystemC.operationC();
      }
  }
  
  public class Client {
      public static void main(String[] args) {
          Facade facade = new Facade();
          facade.operation();
      }
  }
  ```

外观模式- 外观模式（Facade Pattern）：为子系统中的一组接口提供一个一致的界面，定义了一个高层接口，这个接口使得这一子系统更加容易使用。例如：

```
//子系统中的一系列类
class SubSystemA {
    public void methodA() {
        System.out.println("SubSystemA methodA");
    }
}
class SubSystemB {
    public void methodB() {
        System.out.println("SubSystemB methodB");
    }
}
class SubSystemC {
    public void methodC() {
        System.out.println("SubSystemC methodC");
    }
}

//外观类
class Facade {
    private SubSystemA subSystemA;
    private SubSystemB subSystemB;
    private SubSystemC subSystemC;
    public Facade() {
        subSystemA = new SubSystemA();
        subSystemB = new SubSystemB();
        subSystemC = new SubSystemC();
    }
    public void method() {
        subSystemA.methodA();
        subSystemB.methodB();
        subSystemC.methodC();
    }
}

//客户端调用
public class Client {
    public static void main(String[] args) {
        Facade facade = new Facade();
        facade.method();
    }
}
```

## 3. 行为型模式

### 3.1 观察者模式

观察者模式

### 3.2 模板方法模式

观察者模式：

- 定义：一种对象间的一对多的依赖关系，当一个对象状态发生改变时，所有依赖于它的对象都得到通知并被自动更新。
- 实例：一个博客网站，用户可以订阅多个博客作者，当某个作者发表新文章时，所有订阅该作者的用户都会收到通知。

模板方法模式：

- 定义：定义一个操作中的算法的骨架，而将一些步骤延迟到子类中，使得子类可以不改变一个算法的结构即可重定义该算法的某些特定步骤。
- 实例：制作咖啡和茶，它们的制作步骤大致相同，但是具体的实现细节不同，可以使用模板方法模式来实现。

模板方法模式

### 3.3 策略模式

- 模板方法模式
  
  ```
  abstract class Game {
      abstract void initialize();
      abstract void startPlay();
      abstract void endPlay();
  
      //模板
      public final void play(){
  
         //初始化游戏
         initialize();
  
         //开始游戏
         startPlay();
  
         //结束游戏
         endPlay();
      }
  }
  
  class Cricket extends Game {
  
      @Override
      void endPlay() {
         System.out.println("Cricket Game Finished!");
      }
  
      @Override
      void initialize() {
         System.out.println("Cricket Game Initialized! Start playing.");
      }
  
      @Override
      void startPlay() {
         System.out.println("Cricket Game Started. Enjoy the game!");
      }
  }
  
  class Football extends Game {
  
      @Override
      void endPlay() {
         System.out.println("Football Game Finished!");
      }
  
      @Override
      void initialize() {
         System.out.println("Football Game Initialized! Start playing.");
      }
  
      @Override
      void startPlay() {
         System.out.println("Football Game Started. Enjoy the game!");
      }
  }
  
  public class TemplatePatternDemo {
      public static void main(String[] args) {
  
         Game game = new Cricket();
         game.play();
         System.out.println();
         game = new Football();
         game.play();      
      }
  }
  ```

- 策略模式
  
  ```
  public interface Strategy {
      public int doOperation(int num1, int num2);
  }
  
  public class OperationAdd implements Strategy{
      @Override
      public int doOperation(int num1, int num2) {
         return num1 + num2;
      }
  }
  
  public class OperationSubtract implements Strategy{
      @Override
      public int doOperation(int num1, int num2) {
         return num1 - num2;
      }
  }
  
  public class OperationMultiply implements Strategy{
      @Override
      public int doOperation(int num1, int num2) {
         return num1 * num2;
      }
  }
  
  public class Context {
      private Strategy strategy;
  
      public Context(Strategy strategy){
         this.strategy = strategy;
      }
  
      public int executeStrategy(int num1, int num2){
         return strategy.doOperation(num1, num2);
      }
  }
  
  public class StrategyPatternDemo {
      public static void main(String[] args) {
         Context context = new Context(new OperationAdd());    
         System.out.println("10 + 5 = " + context.executeStrategy(10, 5));
  
         context = new Context(new OperationSubtract());      
         System.out.println("10 - 5 = " + context.executeStrategy(10, 5));
  
         context = new Context(new OperationMultiply());    
         System.out.println("10 * 5 = " + context.executeStrategy(10, 5));
      }
  }
  ```

策略模式

### 3.4 命令模式

- 策略模式
  
  策略模式定义了一系列算法，将每个算法封装起来，并且使他们之间可以相互替换。策略模式让算法独立于使用它的客户端而独立变化。
  
  示例：假设有一个商场，有不同的打折策略，如满100减20元，满200减50元等。我们可以使用策略模式设计一个打折策略类，将不同的打折策略封装在不同的类中，客户端可以根据需要选择不同的打折策略。

- 命令模式
  
  命令模式将请求封装成对象，从而使你可以用不同的请求对客户进行参数化，对请求排队和记录请求日志，以及支持可撤销的操作。
  
  示例：假设有一个遥控器，有不同的按钮可以控制不同的电器，如电视、音响等。我们可以使用命令模式设计一个遥控器类，将不同的按钮对应不同的命令类，客户端可以根据需要选择不同的按钮控制不同的电器。同时，我们还可以记录客户端的操作日志，并支持撤销操作。

命令模式

### 3.5 职责链模式

- 命令模式
  
  示例：假设我们要设计一个遥控器，有多个按钮，每个按钮可以控制不同的电器设备。我们可以使用命令模式来实现这个遥控器。具体实现如下：
  
  ```
  // 命令接口
  public interface Command {
      void execute();
  }
  
  // 具体命令类
  public class LightOnCommand implements Command {
      private Light light;
  
      public LightOnCommand(Light light) {
          this.light = light;
      }
  
      public void execute() {
          light.on();
      }
  }
  
  // 接收者类
  public class Light {
      public void on() {
          System.out.println("灯打开了");
      }
  
      public void off() {
          System.out.println("灯关闭了");
      }
  }
  
  // 调用者类
  public class RemoteControl {
      private Command command;
  
      public void setCommand(Command command) {
          this.command = command;
      }
  
      public void buttonPressed() {
          command.execute();
      }
  }
  
  // 客户端代码
  public class Client {
      public static void main(String[] args) {
          Light light = new Light();
          Command lightOnCommand = new LightOnCommand(light);
          RemoteControl remoteControl = new RemoteControl();
          remoteControl.setCommand(lightOnCommand);
          remoteControl.buttonPressed();
      }
  }
  ```

- 职责链模式
  
  示例：假设我们要设计一个请假系统，员工可以提交请假申请，需要经过部门经理、总经理、人事部门等多个人的审批才能最终确定是否批准。我们可以使用职责链模式来实现这个请假系统。具体实现如下：
  
  ```
  // 抽象处理者
  public abstract class Approver {
      protected Approver successor;
  
      public void setSuccessor(Approver successor) {
          this.successor = successor;
      }
  
      public abstract void processRequest(LeaveRequest request);
  }
  
  // 具体处理者1
  public class DepartmentManager extends Approver {
      public void processRequest(LeaveRequest request) {
          if (request.getDays() <= 3) {
              System.out.println("部门经理审批通过");
          } else {
              if (successor != null) {
                  successor.processRequest(request);
              }
          }
      }
  }
  
  // 具体处理者2
  public class GeneralManager extends Approver {
      public void processRequest(LeaveRequest request) {
          if (request.getDays() <= 10) {
              System.out.println("总经理审批通过");
          } else {
              if (successor != null) {
                  successor.processRequest(request);
              }
          }
      }
  }
  
  // 具体处理者3
  public class HRDepartment extends Approver {
      public void processRequest(LeaveRequest request) {
          if (request.getDays() > 10) {
              System.out.println("人事部门审批通过");
          } else {
              System.out.println("请假申请被拒绝");
          }
      }
  }
  
  // 请假申请类
  public class LeaveRequest {
      private int days;
  
      public LeaveRequest(int days) {
          this.days = days;
      }
  
      public int getDays() {
          return days;
      }
  }
  
  // 客户端代码
  public class Client {
      public static void main(String[] args) {
          Approver departmentManager = new DepartmentManager();
          Approver generalManager = new GeneralManager();
          Approver hrDepartment = new HRDepartment();
          departmentManager.setSuccessor(generalManager);
          generalManager.setSuccessor(hrDepartment);
          LeaveRequest request1 = new LeaveRequest(2);
          departmentManager.processRequest(request1);
          LeaveRequest request2 = new LeaveRequest(5);
          departmentManager.processRequest(request2);
          LeaveRequest request3 = new LeaveRequest(12);
          departmentManager.processRequest(request3);
      }
  }
  ```

职责链模式

### 3.6 状态模式

- 职责链模式
  
  职责链模式指的是将请求从一个对象传递到另一个对象的过程中，每个对象都有机会处理请求或将其传递给下一个对象。这种模式可以避免请求的发送者和接收者之间的耦合关系，从而使多个对象都有机会处理请求。

- 状态模式
  
  状态模式指的是当一个对象的状态发生改变时，其行为也会发生改变。这种模式可以将一个对象的状态和行为封装起来，从而使得对象的状态可以独立于其行为而变化。例如，一个订单对象的状态可能包括“待付款”、“已付款”和“已发货”，不同状态下订单的行为也会有所不同。表格：
  
  | 状态  | 行为       |
  | --- | -------- |
  | 待付款 | 显示支付按钮   |
  | 已付款 | 显示发货按钮   |
  | 已发货 | 显示确认收货按钮 |

状态模式

### 3.7 访问者模式

- 状态模式
  
  状态模式主要解决的是对象在多种状态转换时，需要对外输出不同的行为的问题。状态和行为是一一对应的，状态之间可以相互转换。
  
  示例：一个电梯有多种状态（开门、关门、上行、下行、停止），在不同的状态下，按下不同的按钮会有不同的响应。

- 访问者模式
  
  访问者模式可以将数据结构与数据操作分离，使得数据结构和数据操作可以独立地变化。访问者模式适用于数据结构相对稳定的系统，而对于数据结构易于变化的系统，访问者模式则不适用。
  
  示例：一个商场有多个店铺，需要对每个店铺进行不同的打折操作，使用访问者模式可以将打折操作和店铺分离开来，方便管理。

访问者模式

### 3.8 中介者模式

中介者模式

### 3.9 解释器模式

- 中介者模式
  
  中介者模式是一种行为型设计模式，它允许你将复杂的系统组件间的通信方式变成中心化的方式。通过这种方式，系统组件无需显式地相互引用，从而使其松散耦合并且易于独立地修改、扩展和复用。一个经典的例子是机场调度系统，其中调度员就是中介者，负责协调飞机、跑道、塔台等各个组件的通信。

- 解释器模式
  
  解释器模式是一种行为型设计模式，它定义了一种语言语法的表示，并定义了一个解释器来解释这个语言中的语句。这种模式通常用于编写编译器和解释器。一个经典的例子是数学表达式求值，其中解释器可以解释表达式中的操作符和操作数，从而得出表达式的结果。

解释器模式### 3.1 观察者模式

- 观察者模式是一种对象行为型模式，其目的是定义对象间一种一对多的依赖关系，以便当一个对象的状态发生改变时，所有依赖于它的对象都得到通知并自动刷新。

- 观察者模式的角色：
  
  - 抽象主题（Subject）：被观察的对象，它把所有观察者对象的引用保存在一个集合中，并提供增加和删除观察者对象的接口。
  - 具体主题（ConcreteSubject）：具体的被观察者对象，当它的状态发生改变时，向它的观察者们发出通知。
  - 抽象观察者（Observer）：抽象观察者角色，它定义了一个更新接口，使得在得到主题通知时更新自己。
  - 具体观察者（ConcreteObserver）：具体观察者角色，实现抽象观察者角色所要求的更新接口，以便进行具体的更新操作。

- 解释器模式
  
  - 解释器模式是一种对象行为型模式，它定义了一套语言文法规则并用该文法规则解释语言中的句子。它是一种解释型模式，用于解释一些固定格式的表达式或者语句。
  - 解释器模式的角色：
    - 抽象表达式（AbstractExpression）：声明一个抽象的解释操作，这个接口为抽象语法树中所有的节点所共享。
    - 终结符表达式（TerminalExpression）：实现与文法中的终结符相关联的解释操作。
    - 非终结符表达式（NonterminalExpression）：为文法中的非终结符实现解释操作。
    - 环境（Context）：包含解释器之外的一些全局信息。

## 以上是Java设计模式的汇总，包括创建型模式、结构型模式以及行为型模式。每个模式都有对应的介绍和示例，可以帮助软件工程师更好的理解和应用设计模式。
