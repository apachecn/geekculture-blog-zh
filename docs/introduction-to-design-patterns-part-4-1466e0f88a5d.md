# 设计模式介绍:第 4 部分

> 原文：<https://medium.com/geekculture/introduction-to-design-patterns-part-4-1466e0f88a5d?source=collection_archive---------7----------------------->

![](img/2a5a016b00e2557f55e7923849df4304.png)

Photo by [amin khorsand](https://unsplash.com/@hero92?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/collections/335434/coding?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

在这篇文章中，我们将讨论行为设计模式。在本系列的最后一期[中，我们已经看到了带有示例的结构设计模式。正如在](/@myac.abhijit/introduction-to-design-patterns-part-2-c01ef87ac74e)[第一部分中所讨论的，](/@myac.abhijit/introduction-to-design-patterns-part-2-c01ef87ac74e)这些模式通常处理对象之间的通信。让我们开始吧。

## **责任链模式**

## 问题

在某些情况下，我们面临某些情况，这需要一步一步的方法来获得解决方案，或者事情变得太复杂或紧密耦合，因为每个责任只分配给一个或两个对象。例如，让我们说，我们去签发护照。它由几个步骤组成，从申请预约、获得住所到生物特征和背景验证。在所有这些步骤都完成并满足之后，我们就可以拿到护照了。如果我们在任何一步被拒绝，我们不会进入下一步。如果我们需要在应用程序中复制这种行为呢？

## 解决办法

责任链模式在应用程序代码中做完全相同的事情。一步一步的进展有助于保持一个简单的、单一负责的和松散耦合的方法。因为每个步骤都由一个单独的对象处理。这些单独的对象中的每一个都被称为处理程序。

处理程序通常有两个公共方法和一个对下一步处理程序的引用。一种方法用于设置对下一个处理程序的引用，另一种是“执行”或“处理”方法，用于触发该处理程序的主要功能。当我们从外部添加下一个处理程序的引用时，它可以在运行时被改变。

所有的处理程序只实现一个接口来实现成功的通信。一些功能在一些处理程序中可能是通用的，这些处理程序通常在一个被称为“基本处理程序”的基类中定义，该基类由被称为具体处理程序的实际处理程序对象扩展。

## 代码片段

```
public interface IHandler
{
      public void SetNextHandler(IHandler handler);
      public void ExecuteHandling();
}

public class BaseHandler : IHandler
{
       private IHandler NextHandler;

       public virtual void ExecuteHandling()
       {
       }

       public void SetNextHandler(IHandler handler)
            => NextHandler = handler;
}

public class AppointmentConfirmation : BaseHandler
{
        private Appointment Appointment;    // Object

        public AppointmentConfirmation(Appointment appointment) 
        {
              Appointment = appointment;
        }

        public override void ExecuteHandling()
        {
                if(!valid())
                     print("Appointment Not Confirmed");
                else
                {
                     print("Appointment confirmed");
                     NextHandler.ExecuteHandling();
                }
         }

         private bool valid()
         {  // do something
         } 
}

public class DocumentVerification : BaseHandler
{
        private Document Document;    // Object

        public AppointmentConfirmation(Document document) 
        {
              Document = document;
        }

        public override void ExecuteHandling()
        {
                if(!valid(Document))
                     print("Document verification failed");
                else
                {
                     print("Documents Verified");
                     NextHandler.ExecuteHandling();
                }
         }

         private bool valid()
         {  // do something
         } 

}

public class PassportPublisher : BaseHandler
{

        public override void ExecuteHandling()
        {
              print("Publishing Passport");
        }

}

// Application

var AppointmentConfirmer = new AppointmentConfirmation(appointment);
var DocumentVerifier = new DocumentVerification(document);
var Publisher = new PassportPublisher();

AppointmentConfirmer.SetNextHandler(DocumentVerifier);
DocumentVerifier.SetNextHandler(Publisher);
```

## **观察者模式**

## 问题

我们考虑这个问题，我们正在等待一个产品可以在亚马逊这样的电子购物网站上买到。在这种情况下，我们作为买家将继续访问网站，直到产品可用，这将导致几次失败的尝试，并失去努力。如果电子购物网站有一个通知机制，可以通知一些事件或产品可用性，并且我们订阅这些通知，那么这个问题就可以解决。现在，如果订阅者收到所有类型的通知，就会收到垃圾邮件，并且很难跟踪真正需要的通知。

## 解决办法

为了解决这个问题，观察者或订阅者模式引入了两个实体，一个是发布者或主题，负责发送通知，另一个是订阅者，负责监听它所订阅类型的通知。发布者实现了一个接口，该接口具有一些功能，如添加新订阅者、删除订阅者和通知订阅者的方法。Subscriber 类有一个 listen 或 subscribe 方法来开始侦听 Publisher。发布者具有不同订户的地图，即订户及其类型。订阅者有 update 方法，这些方法在得到发布者的通知时执行一些特定的活动。

## 代码片段

```
// Listener Interface

public interface IEventlistener
{
    void Update();
}

// Listener 1

public interface IBuyPhone
{
    void Buy();
}

public class PhoneAvailablityListener : IEventlistener , IBuyPhone
{
      public void Buy()
      {
           print("Buy iPhone 14");
      }

      public void Update()
      {
            print("Updating Phone availability Listener");
            Buy();
      }
}

// Listener 2

public interface ISurfShoes
{
    void Surf();
}

public class ShoeAvailablityListener : IEventlistener , ISurfShoes
{
      public void Surf()
      {
           print("Find Sneakers for men");
      }

      public void Update()
      {
            print("Updating Shoe availability Listener");
            Surf();
      }
}

// Publisher Interface

public interface IPublisher
{
      public void AddSubscribers(string type, IEventlistener listener);
      public void RemoveSubscribers(string type);
      public void Notify(IEventlistener[] listeners);
}

public interface ICheckStock
{
     void CheckStockForPhonesAndNotify();
     void CheckStockForShoesAndNotify();
}

public class ShoppeSite: IPublisher, ICheckStock
{
      private IEventlistener[] phoneAvailablityListerners = new IEventlistener[]() { };
      private IEventlistener[] shoeAvailablityListerners = new IEventlistener[]() { };
      private bool ShoeStock;
      private bool PhoneStock;

      public ShoppeSite(bool phoneStock, bool shoeStock)
      {
           ShoeStock = shoeStock;
           PhoneStock = phoneStock;
      }

      public void AddSubscribers(string type, IEventlistener listener)
      {
             if (type == "phone")
                 phoneAvailablityListerners.add(listener);
             else
                 shoeAvailablityListerners.add(listener);

      }

      public void RemoveSubscribers(string type)
      {
             if (type == "phone")
                 phoneAvailablityListerners.remove(listener);
             else
                 shoeAvailablityListerners.remove(listener);

      }

      public void Notify( IEventlistener[] listerners )
      {
              foreach ( listener in listeners )
              {
                   listener.update();
               }
     }

     public void CheckStockForPhonesAndNotify()
     {
           if (PhoneStock)
              Notify(phoneAvailablityListerners);
     }

     public void CheckStockForShoesAndNotify()
     {
           if (ShoeStock)
              Notify(shoeAvailablityListerners);
     }

}
```

## **访客模式**

## 问题

有时我们有不同类的对象，它们已经在复杂的函数和方法中使用。此时由于一些需求，如果我们需要修改或添加这些类的一些功能，我们需要处理大量复杂的代码，这可能会引入一个 bug。此外，很难触及整个代码，因为紧密耦合引入了一个小的变化。

## 解决办法

为了解决这个问题，访问者模式将对象和算法功能分开。这是通过创建 visitor 类来实现的，它具有对象所需的功能。在这些函数中，我们传递对象，所以，visitor 类的函数使用所有的功能和对象的成员值。功能所针对的对象称为元素。其思想是访问者对象访问所有的元素对象，元素对象执行功能。在现实世界的类比中，访问者对象可以被认为是推销员，而访问者携带的元素对象或对象的功能可以被认为是房屋。因此，销售人员或访问者访问每个房屋或元素对象，房屋或对象的所有者购买产品或执行功能。

就像一个推销员有几种产品一样，visitor 对象可能有不同的功能。现在，所有的产品都不适合每个家庭。每个业主都有不同的要求，并据此购买。同样，所有的元素对象不需要执行访问者中的所有函数，它只执行接受元素对象类型的对象的函数。

每个元素对象都有一个接受方法。accept 方法接受访问者，并调用所需的函数传递对象。

## 代码片段

```
// Visitor Interface: Has three types of functions

public interface ISalesmanVisitor
{
      public void visit(ShoeStore store);

      public void visit(ApparelStore store);

      public void Visit(StationaryStore store);
}

// Visitable interface

public interface IVisitableStore
{
       public void accept(ISalesmanVisitor visitor);
}

// Store interfaces

public interface IShoeStore
{
       public void getStock();

}

public interface IApperalStore
{
       public void getStock();

}

public interface IStationaryStore
{
       public void getStock();

}

// Store classes

public class ShoeStore : IVisitableStore, IShoeStore
{
      public int stock;

      public void getStock()
          => print($"Stock for Shoes: {stock}");

      public void accept(ISalesmanVisitor visitor)
           => visitor.visit(this);

      public void update(int stock)
            => this.stock = stock;

} 

public class ApperalStore : IVisitableStore, IApperalStore
{
      public int stock;

      public void getStock()
          => print($"Stock for Clothes: {stock}");

      public void accept(ISalesmanVisitor visitor)
           => visitor.visit(this);

      public void update(int stock)
            => this.stock = stock;

} 

public class StationaryStore : IVisitableStore, IApperalStore
{
      public int stock;

      public void getStock()
          => print($"Stock for Stationary: {stock}");

      public void accept(ISalesmanVisitor visitor)
           => visitor.visit(this);

      public void update(int stock)
            => this.stock = stock;

} 

// Visitor definitions

public class SalesVisitor : ISalesmanVisitor
{
     private int StockUpdate;

     public SalesVisitor(int stockUpdate)
     {
           StockUpdate = stockUpdate;
     }

     public void visit(ShoeStore store)
         => store.update(StockUpdate);

     public void visit(ApparelStore store)
         => store.update(StockUpdate);

     public void visit(StationaryStore store)
         => store.update(StockUpdate);
}

// Application

var stationaryStore = new StationaryStore();
var ShoeStore = new ShoeStore();

var Salesman = new SalesVisitor(20);

Salesman.visit(stationaryStore);
stationaryStore.getStock();

Salesman.visit(ShoeStore);
ShoeStore.getStock();
```

## **模板图案**

## 问题

让我们考虑制动系统的类型:鼓式和盘式。在所有这些类别中，一些功能对于所有类别是相同的，但是一些与机制相关的功能是不同的。现在，如果我们创建两个独立的类，并在每个地方添加函数，将会有大量的冗余，而且添加一个新的类是不可行的，仅仅因为 2-3 个方法是不同的。

## 解决办法

模板模式引入了基类的概念，在我们的例子中，Brakes 可以是一个很好的基类，然后这两个类可以是派生类。现在，模板模式要求在基类中声明公共功能，并为派生类中实现不同的其他函数添加抽象方法。使它们抽象会迫使子类声明它们自己的功能。子类覆盖了那些抽象函数。基类有一个模板方法，它实际上调用了类中的所有其他方法。模板方法处于其最终形式，不能被覆盖。

这还有另一个问题。有时一个特定的子类并不需要所有的函数，如果我们在基类中抽象它，我们就不能创建那个子类的对象，除非我们覆盖它。为了避免这种情况，我们在基类中虚拟了一些函数，但是提供了空的主体。这可以防止方法变得抽象，并且不会停止对象的创建。这些功能被称为可选步骤或方法。

## 代码片段

```
public abstract class BrakingSystem
{
      // Template Methods: Using all the other methods.

      public void ApplyBrakes()
      {
            print("Applying Brakes");

            ApplyBrakePaddle();
            ApplyBrakinMechanism();
            StopTheMovement();
      }

      public void ApplyBrakePaddle()
      {
            print("Applying Brake Paddle");
      }

      // abstract methods

      public abstract void ApplyBrakinMechanism();

      public void StopTheMovement()
      {
             print("Stopping the vehicle movement");
      }
}

public class DrumBrakingSystem: BrakingSystem
{
        // Overriding

        public override void ApplyBrakinMechanism()
        {
                print("Using Drum mechanism");

        }
}
```

## **迭代器模式**

## 问题

问题陈述非常直接。我们经常使用几种复杂的数据结构和类。当我们使用这些类实例的独特的复杂集合时，比如一个映射或一个图，我们所做的迭代的类型，从一个集合到另一个集合是不同的，就像我们遍历树的方式不同于我们对一个图所做的那样。

## 解决办法

为了解决这个问题，迭代器模式告诉从 iterable 集合类中分离迭代器类。迭代器类有一个接口，该接口有类似 getNext()的函数，有助于遍历，iterable 集合有一个 CreateIterator()方法，该方法创建迭代器，该函数必须返回迭代器接口类型。

迭代器对象负责返回当前对象，下一个对象，迭代到下一个对象，获取当前对象的索引等等。

## 代码片段

```
public interface Iiterator
{
      public int Position();
      public IObject GetObject();
      public bool hasMore();
}

public interface IiterableCollection
{
      public Iiterator CreateIterator();
}
```

## 状态模式

## 问题

在某些情况下，我们有一个对象，它的行为会根据对象的状态而改变。例如，如果我们考虑一部手机，当我们打开省电模式时，它的行为会发生变化。现在，如果我们试图保持所有这些行为，在一个特定的类中，它应该有许多属性来保持状态和许多 if-else 条件或切换情况，以相应地切换行为。如果梯子有问题，可能会导致悬挂等问题。

## 解决办法

状态模式建议我们为每个状态创建一个单独的类，并且只在那些类中保存状态智能行为。这些类实现了一个名为 State 的接口，它反映了对象应该执行的功能。而实现接口的所有类都被称为具体状态。每个类定义基于状态有不同的功能实现。现在，客户端不能在不同的时间与不同的对象进行交互，为了解决这个问题，创建了另一个类，它维护一个状态对象类型的数据成员，该数据成员根据情况引用当前的状态对象。这个类称为上下文。上下文实现了一个接口，该接口包含 state 接口中包含的所有函数，除此之外还有一个 stateSetter()，用于设置上下文的状态。这个类中的函数相应地调用被引用的真实状态对象的函数。客户端只与上下文交互。

## 代码片段

```
// State
public interface IPhoneState
{
    public void display();
    public void interact();
    public void maintainBackgroundMethods()
}// Concrete States
public class NormalMode()
{
    public void display()
    {
          print("Providing High quality display: requesting high data");
    }
    public void interact()
    { 
          print("Providing smooth interactions using buffers and background processes");
    }
    public void maintainBackgroundMethods()
    {
          print("Maintain all background processes: Necessary and for smooth interactions");
    }
}public class PowerSavingMode()
{
    public void display()
    {
          print("Providing medium quality display: requesting less data");
    }
    public void interact()
    { 
          print("Providing optimum interactions using less buffers and background processes");
    }
    public void maintainBackgroundMethods()
    {
          print("Maintain Necessary background processes only");
    }
}// Context public interface IContextSetter
{
      public void StateSetter(IPhoneState phonestate);
}public class PhoneContext : IPhoneState,IContextSetter
{
      private IPhoneState Phonestate;

      public void StateSetter(IPhoneState phonestate)
            => Phonestate = phonestate;

      public void display()
            => Phonestate.display(); public void interact()
            => Phonestate.interact(); public void maintainBackgroundMethods()
            => Phonestate.maintainBackgroundMethods()
}
```

## 命令模式

## 问题

这种模式我们都无意中使用过很多次。让我们考虑发送一个 HTTP 请求。现在，假设您正在从一种不支持 HttpClient 类的非常原始的语言发送一个请求。如果我们创建一个名为 request 的类并用它发出每个请求，我们必须传递许多参数并使用函数重载。否则，我们可以创建一个基类 request 和子类 SendRequest、PostRequest、PatchRequest 等来处理每种类型。但是，第二种类型将需要创建许多子类，并且将非常紧密地耦合一切，因为基类中的微小变化将破坏一切。

## 解决办法

现在，为了处理这个问题，在所有常用的语言中，我们有一个类 HttpRequestMessage (C#)或类似的，它将请求的细节封装在一个对象中。这正是模式所说的。该对象拥有关于请求的所有细节，包括请求的类型、授权、头、主体内容等。

这个请求对象作为一个参数传递给 httpclient 对象的 send()功能，以发出请求。这里，HttpClient 对象被称为接收者，因为它接收请求对象。如果我们没有为请求创建封装的类，那么不同的请求将需要不同的参数，例如，post 请求不需要主体，因此对于接收者来说，必须有几个具有不同参数的函数。使用请求对象，只有一个函数可以用来处理所有的请求类型，

## 中介模式

## 问题

让我们考虑只有一条跑道的空中交通管制来理解这一点。A 航班和 B 航班正在等待着陆，而 C 航班和 D 航班已经准备好登机。如果这些航班试图相互作用，决定一个顺序，就会超级混乱。同样，在代码类比中，如果几个对象试图相互交互，那将是一片混乱，并导致非常紧密的耦合。

## 解决办法

中介体模式表明对象之间根本不应该交互。它们都应该向一个中介对象报告，而这个中介对象又应该简化和推进交互。这种模式被广泛使用，因为它促进了松散耦合。在现实世界的类比中，ATC 也保持着中介者的角色。中介是一个接口，它陈述控制交互的功能。实现中介器的类称为具体中介器。而对象报告，这里的航班被称为组件。

现在，在真实的情况下，飞行不需要知道彼此的存在。类似地，在实现中介模式时，对象不需要知道其他对象。

## 代码片段

```
public interface IMediator
{     
       public bool IsLandingPossible();
       public void LandingSucceeded();
       public bool IsTakeOffPosssible();
       public void TakeOffSucceeded();
}public class ATCMediator
{
      private bool isRunwayBusy = true;        public bool IsLandingPossible()
      {
           if(!isRunwayBusy)
           {
                isRunwayBusy = true;
                return true;
           }
           return false;
      }       public bool IsTakeOffPosssible()
      { 
           if(!isRunwayBusy)    
           {
                isRunwayBusy = true;
                return true;
           }
           return false;
      }

      public void LandingSucceeded()
             => isRunwayBusy = false;

      public void TakeOffSucceeded()
             => isRunwayBusy = false;
}public class Flight
{
     private IMediator Mediator; public Flight(IMediator mediator)
     {
            Mediator = mediator;
     }

     public void RequestLanding()
     {
          if(Mediator.IsLandingPossible())
               Land();
          else
              GoAround();
     }

     public void RequestTakeOff()
     {
         if(Mediator.IsTakeOffPosssible())
               TakeOff();
         else
               Wait();
     } private void Land()
       => Mediator.LandingSucceeded(); private void TakeOff()
       => Mediator.TakeOffSucceeded();
}ATCMediator atc = new ATCMediator();Flight flightA = new Flight(atc);
Flight flightB = new Flight(atc);flightA.RequestLanding();
```

## 战略模式

## 问题

有时候一个特定的任务可以用多种方式完成。例如，我们想从目的地 A 到目的地 B，我们可以通过三种方式，航空，公路和铁路。现在，每一种方式都是一种策略。最好的例子是谷歌地图提供的路线建议。同样，如果我们试图用一个单独的类来做这件事，这将会非常困难，因为它将包含大量的条件和复杂性。

## 解决办法

策略模式建议我们为每个策略维护单独的类，它封装了我们在该策略中应该遵循的行为。现在，所有的策略类都应该实现一个特定的接口，该接口包含要定义的函数，例如，在我们的例子中，函数 travel()可以是该接口的一部分。这个界面叫做策略。实现策略接口的类被称为具体策略。

这里的问题变成了，我们如何在不同的点上与不同的策略互动。为此，我们有一个名为 Context 的类，它有一个策略接口类型的私有成员。它存储一个策略，并实现策略接口。context 类的功能只是调用特定成员策略的功能。客户端可以与上下文交互。

## 代码片段

```
// Strategy
public interface ITravelStrategy
{
     public void travel();
}
public class TravelByAir : ITravelStrategy
{
     public void travel()
     {
         print("Took a flight");
     }
}
// Concrete strategiespublic class TravelByRoad : ITravelStrategy
{
     public void travel()
     {
         print("Took a bus/cab");
     }
}
public class TravelByRail : ITravelStrategy
{
     public void travel()
     {
         print("Took a train");
     }
}// Context classpublic class TravelContext: ITravelStrategy
{
     private ITravelStrategy TravelStrategy;

     public TravelContext(ITravelStrategy travelStrategy)
     {
           TravelStrategy = travelStrategy;
     }

     public void travel()
         => TravelStrategy.travel();}// Applicationvar travel = new TravelByAir();
var travelContext = new TravelContext(travel);travelContext.travel();
```

## 纪念品图案

Memento 模式用于解决一个非常简单的问题。在我们的日常生活中，我们使用几个有撤销选项的应用程序。它已经成为几乎所有应用程序中非常必要和期望的功能。现在，我们有没有想过，这是怎么发生的？使用我们日常应用的面向对象编码将是非常困难的，因为它需要存储对象状态，并设置其他依赖对象，记住所有的封装原则。

Memento 提出了一个解决方案，使用 3 个类，一个 Originator 类，基本上是需要存储其实例状态的类。一个实际上设置和存储值的纪念品实例，以及一个管理纪念品的保管人。每当需要存储发起者对象的状态时，照管者要求发起者用其当前状态值创建纪念品。每当需要撤销操作时，发起者创建纪念品，并将其传递给维护它并传递它的看护者，以进行重置。

现在，当发起者自己创建它自己的纪念品时，不存在封装的问题，因为对象可以访问它自己的方法和成员，而作为中间人的看护者处理所有事情，降低了整个事情的复杂性。

## 结论

在本文中，我们已经看到了所有带有示例的行为设计模式。关于 GoF 设计模式的系列文章到此结束。

快乐阅读！