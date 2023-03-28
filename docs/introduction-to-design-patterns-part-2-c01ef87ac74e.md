# 设计模式介绍:第 2 部分

> 原文：<https://medium.com/geekculture/introduction-to-design-patterns-part-2-c01ef87ac74e?source=collection_archive---------11----------------------->

![](img/2a5a016b00e2557f55e7923849df4304.png)

Photo by [amin khorsand](https://unsplash.com/@hero92?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/collections/335434/coding?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

在这个系列的这一部分，我们将看看创造性的设计模式。正如在[最后一部分](/@myac.abhijit/introduction-to-design-patterns-part-1-de750d72051f)中提到的，我们知道有 4 个对象模式和 1 个类模式。让我们来看一看其中的每一个

## **独生子女模式**

## **问题**

有时我们只需要创建一个类的实例。例如，对于一个数据库连接对象，在整个解决方案中，我们需要一个用于所有用途的一致对象。

## 解决办法

为解决这个问题而设计的类被称为单例类，它本质上利用了“静态”关键字属性。其思想是用一个类类型的静态对象作为类的数据成员、一个私有构造函数和一个返回类实例的静态方法。

在每次调用创建类的新实例时，我们调用静态方法，该方法有一个空检查，在实例化之前检查数据成员是否为空。在第一次调用时，成员值为 null，因此创建了一个实例并将其分配给成员，我们返回成员值。从第二次调用开始，静态成员不为空(因为它是静态的，所以它不是任何对象的私有成员)，我们返回第一次调用时创建的相同值。

## 代码片段

```
public class SingletonDemo
{
      private static SingletyonDemo instance;

      private SingletonDemo() { };

      public static SingletonDemo GetInstance()
      {
            if( SingletonDemo.instance == Null)
            {
                  SingletonDemo.instance = new SingletonDemo();
            }
            return SingletonDemo.instance;
       }
}
```

## **构建器模式**

## 问题

有时我们必须构建具有多个属性的复杂对象，这些属性可能有不同的值。例如，如果我们必须建造一所房子，它将有墙壁，地板，屋顶，房间等。现在，根据房子的类型，别墅或小木屋，可能会有花园和游泳池。如果我们必须将所有这些变量放入类的构造函数中，它将变得庞大而笨拙，并且根本不可扩展。此外，有时只要改变执行的顺序或构造，结构就会改变，在这种情况下，我们将需要定义单独的构造函数，这将导致创建大量的构造函数。因此，构建器模式作为一种解决方案被引入。

## 解决办法

Builder 模式引入了两个独立的类，而不是我们需要创建其对象的复杂类，即 builder 类和 director 类。builder 类有一个要构建的复杂类的实例和 N 个用于构建复杂类对象的各个属性的方法，最后还有一个返回所创建对象的 GetResult()方法。

由于每个属性的创建都是用不同的函数来处理的，我们可以将各自的参数传递给单独的函数，这样会更简洁。此外，如果一个新的属性必须被添加，我们可以只添加一个新的函数，它可以被扩展，不需要一个单独的构造函数的变化。在某些情况下，所有的属性都不是必需的，在这种情况下，我们不会调用那个特定的函数，保持创建过程的模块化。

## 代码片段

```
public class House
{
    private int rooms;
    private int floorSpace;
    private string paint;
    private bool HasGarden;
    private bool hasPool;

    public void SetRooms(int rooms)
    { this.rooms = rooms; }

    public void SetFloorSpace(int floorSpace)
    { this.floorSpace = floorSpace; }

    public void SetPaint(string paint)
    { this.paint = paint; }

    public void SetGarden(bool HasGarden)
    { this.HasGarden = HasGarden; }

    public void SetPool(bool hasPool)
    { this.hasPool = hasPool; }

}

public interface IBuilder
{
   void BuildRooms(int rooms);
   void BuildFloor(int floorSpace);
   void Paint(string paint);
   void BuildGarden(bool HasGarden);
   void BuildPool(bool hasPool);
   House GetResults();
}

public class HouseBuilder : IBuilder
{
    private House instance;

    public HouseBuilder()
    {    instance = new House();  }

    public void BuildRooms(int rooms)
       => instance.SetRooms(rooms);

    public void BuildFloor(int floorSpace)
       => instance.SetFloorSpace(floorSpace);

    public void Paint(string paint)
       => instance.SetPaint(paint);

    public void BuildGarden(bool HasGarden)
       => instance.SetGarden(HasGarden);

    public void BuildPool(bool hasPool)
       => instance.SetPool(hasPool);

    public House GetResults()
       => instance;
}

public class main()
{
    HouseBuilder builder = new HouseBuilder();
    builder.BuildRooms(2);
    builder.BuildFloor(500);
    builder.Paint("Blue");
    builder.BuildGarden(true);
    builder.BuildBool(true);

    House house = builder.GetResults();

}
```

## 延长

我们已经看到了构建器模式是如何工作的，但是使用另一个名为 Director 的类可以进一步简化它。Director 可能被认为是维护构建调用流和构建者创建的对象类型的构建者的领导者。

## **工厂模式**

## 问题

让我们考虑一个人有两辆自行车的情况，一辆运动型自行车和一辆标准的街道自行车。这两种自行车有不同的性能和骑指令取决于发动机和齿轮清单。这两种自行车具有相似的功能，因此它们可以从相同的抽象基类“Bike”扩展，或者可以实现相同的“IBike”接口。一个 Person 类实例可以有 1 个 IBike 实例来实现这两种类型，但是每次都使用 new 关键字会很麻烦，所以引入了工厂模式。

## 解决办法

工厂模式添加了一个工厂类函数，该函数只返回子类类型的对象。使用同一个函数我们可以实例化不同的类，并使用相应的行为和函数。

如果我们想添加一辆像 cruiser 这样的新自行车，使用这种模式将非常容易添加。

## 代码片段

```
public interface IBike
{
     public void Accelerate();
}

public class SportsBike: IBike
{
     private int Mileage;
     private int MaxSpeed;
     private int Gears;

     public SportsBike(int mileage, int speed, int gears)
     {
         Mileage = mileage;
         Speed = speed;
         Gears = gears;
     }

     public static IBike Create()
         => new SportsBike(20, 240, 6)

     public void Accelerate()
     {
           print($"0-20 -> Gear 1");
           print($"20-40 -> Gear 2");
           print($"40-60 -> Gear 3");
           print($"60-90 -> Gear 4");
           print($"90-130 -> Gear 5");
           print($">130 -> Gear 6");
     }
}

public class StandardBike: IBike
{
     private int Mileage;
     private int MaxSpeed;
     private int Gears;

     public SportsBike(int mileage, int speed, int gears)
     {
         Mileage = mileage;
         Speed = speed;
         Gears = gears;
     }

     public static IBike Create()
         => new SportsBike(45, 140, 5)

     public void Accelerate()
     {
           print($"0-20 -> Gear 1");
           print($"20-40 -> Gear 2");
           print($"40-60 -> Gear 3");
           print($"60-80 -> Gear 4");
           print($"80-140 -> Gear 5");
     }
}

public class BikeFactory
{
     private static IBike bike;

     public static IBike BikeFactory(string type)
     {
           switch(type)
           {
               case "Sports":
                  bike = new SportsBike.Create();
                  break;

               case "Standard":
                  bike = new StandardBike.Create();
                  break;

               default:
                  break;
          }
     }
}

var bike = BikeFactory.BikeFactory("Sports");
bike.accelerate();
```

## **原型图案**

## 问题

有时有一些类，其实例的属性变化有限。在这种情况下，我们需要相同对象的多个副本。让我们考虑汽车发动机，梅赛德斯为一系列汽车制造类似的发动机，这些发动机后来进行了改装。现在，为了创建精确的引擎副本，我们需要传递完全相同的参数，我们从我们克隆的对象中获取数据成员，但有时它们是私有的，不能从外部访问。同样，为了实例化一个复制对象，我们需要原始对象是其实例的类，所以我们变得依赖于该类，这引入了紧耦合。此外，有时对象是作为接口的实例传递的，在这种情况下，我们不能正确地实现类。

## 解决办法

为了解决这个问题，我们向需要克隆自身的实际对象添加了克隆功能。作为成员方法的函数可以访问对象的私有成员。对于层次类，我们将一个抽象的 clone()函数放在基类中，并在子类中覆盖。具有 Clone()的类称为原型。因为这样的实例数量是固定的，所以它们被创建并存储在一个列表中，以加速克隆过程，而不是在以后创建。该数组称为原型注册表。

## 代码片段

```
public interface IShape
{
      public int GetArea();
      public IShape Clone();
}

public class Square : IShape
{
      private int side;

      public Square( int side )
      {
            this.side = side;
      }

      public int GetArea()
         => side * side;

      public IShape Clone()
         => new Square(side);
}

public class Circle : IShape
{
      private int radius;
      private const pi = 3.14;

      public Square( int radius )
      {
            this.radius = radius;
      }

      public int GetArea()
         => radius * radius * pi;

      public IShape Clone()
         => new Circle(radius);
}

var circle = new Circle(3);
var circle2 = circle.Clone()
```

## 抽象工厂模式

## 问题

让我们用一个例子来理解这一点，假设一家跨国公司正在建立生产汽车的工厂。所以，通常一家工厂生产掀背车、SUV 和轿车。但是现在，每个国家都有一套关于汽车排放、引擎和驾驶座位排列的独立规则，例如:印度是右座驾驶，而美国是左座驾驶。如果我们在这里也使用子类和继承，这将导致类的爆炸式增长，以及冗余。

## 解决办法

抽象工厂模式通过在我们旧的学校工厂模式上增加另一个抽象层来解决这个问题。它指出，我们应该有一个工厂接口，根据配置或需求返回工厂类的类型。比如说，接口车定义为:

```
public interface ICar
{
     public void Start();
     public void Stop();
     public void GetPerformanceDetails();
     public void GetUsageDetails();
     public ICar CreateCar();}
```

现在，所有三类汽车，即轿车、SUV 和掀背车都实现了这些接口，然后我们可以有一个工厂类来根据需要返回对象，这就是我们在工厂类中看到的。在这种情况下，我们将另外拥有另一个接口，它实际上被定义为

```
public interface ICarFactory
{
    public ICarFactory CreateFactory(Location location);
}
```

该接口由每个工厂为特定区域实现，如 IndiaCarFactory 和 USACarFactory 类，它们依次返回对象。

## 结论

在这篇文章中，我们已经看到了创造性的设计模式。在下一部分中，我们将了解一下结构设计模式。

快乐阅读！