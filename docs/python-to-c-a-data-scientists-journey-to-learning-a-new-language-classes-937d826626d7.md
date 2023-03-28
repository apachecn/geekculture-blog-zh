# Python 到 C++，数据科学家学习新语言的旅程——类

> 原文：<https://medium.com/geekculture/python-to-c-a-data-scientists-journey-to-learning-a-new-language-classes-937d826626d7?source=collection_archive---------25----------------------->

![](img/890736ea088de4cf94031ae66343a512.png)

## 介绍

勇敢的读者们，欢迎回到 Python 到 C++系列的另一部分。在上次会议中，我们讨论了 Python 和 C++中函数的重要性和用法。今天，我们将通过深入研究类的世界来扩展这个想法。我们将回顾类的一些基本用例，然后通过在每种语言中组合一个可用于开发 RPG 角色的类来创建我们自己的例子。

## 班级

在上一期中，我提到了函数可以用来将我们希望在整个程序中频繁重复的代码行组合在一起。一个类是一组协同工作以完成一个共同目标的函数。例如，在我们今天的例子中，我们将创建一个基于许多不同因素创建角色的类:我们将有一个角色名字的函数，一个角色种族的函数，一个角色专业的函数，等等。

在 Python 中，创建类的方法是首先定义我们的类，然后是我们希望的类的名称；命名约定遵循每个单词的大小写，单词之间没有空格:

```
class ClassName():
```

我们创建的每个类都必须从初始化函数开始。当调用和创建新的对象或类时，该函数将值初始化为任何重要的数据。这个函数将包含我们希望在整个课程中使用的任何变量。它被定义为我们创建的任何其他函数，但必须具有名称 __init__ 以允许计算机识别这是初始化函数。在括号中必须包含 self 属性，这允许识别整个类中的类变量和数据。在 __init__ 函数体中，我们包含了我们希望贯穿始终的类变量；这些变量必须使用 self 进行实例化。方法如下所示:

```
def __init__(self, att2, att3, att4):
    self.att2 = att2
    self.att3 = att3
    self.att4 = att4
```

类的其余部分由需要包含的任何函数组成；同样，self 必须作为每个函数的属性传递，以允许识别类的属性。在这些函数中，可以再次使用 self 调用类变量或对象属性。方法:

```
def func2(self):
    print(self.att2)
    return self.att3 + self.att4
```

类属性中的值也可以通过使用 self 调用属性来更改。方法，并将其设置为感兴趣的新值:

```
def func3(self):
    if self.att2 > self.att3:
        self.att4 = self.att2
    else:
        self.att4 = self.att3
```

在 C++中，类或对象是以类似的方式创建的，方法是将对象定义为一个类，然后给该类一个名称；然后，类的主体必须用花括号括起来:

```
class ClassName
{
    <class body>
};
```

C++类中的“初始化函数”通过将类设置为 public、private 或 protected 来表示。这些是访问说明符，告诉计算机属性和方法(类函数)是否可以在类外访问。Public 允许在类外部访问；private 不允许在类外部访问；protected 只允许在继承的实例中访问类的外部，这一点我们将在后面讨论。如果我们希望类的某些部分保持私有，其余部分保持公共，我们可以指定多个访问说明符。该访问说明符的主体将包括该类的属性和方法，这些属性和方法的设置类似于任何其他变量或函数；请注意，与 python 不同，在方法中设置类属性时，我们仍然必须返回属性，而在 python 中，我们不需要指定这样的返回:

```
public: std::string att1;
    std::string att2;
    int att3;
    double att4;
    bool att5; std::string setAtt1()
    {
        std::cout << “What is att1?”;
        std::cin >> att1;
        return att1;
    }; std::string setAtt2()
    {
        std::cout << “What is att2?”;
        std::cin >> att2;
        return att2;
    }; int att3PlusAtt4()
    {
        att3 = 5;
        att4 = 10.6;
        return att3 + att4;
    }; bool setAtt5()
    {
        if (att3PlusAtt4() > 5)
        {
            att5 = true;
        }
        else
        {
            att5 = false;
        }
    };private:
    std::string att6 = “CLASSIFIED”;
    int att7 = 42;
```

## 调用和使用类

在 Python 中，可以通过简单地实例化对象调用的新变量来调用类:

```
myClass = ClassName(“hello readers”, 1, 2)
```

为了访问这个新对象中的属性和方法，我们使用了点方法:

```
var = myClass.func2()
myClass.func3()
var2 = myClass.att4
print(var2) # will print out a boolean value
```

在 C++中，类的调用方式非常相似。首先调用这个类，然后调用新对象的名称。

```
int main()
{
    ClassName newClass;
}
```

就像在 Python C++中，类属性和方法是使用点方法调用的:

```
newClass.setAtt1();
newClass.setAtt2();
newClass.setAtt4();
newClass.setAtt5();
std::cout << newClass.att1 << std::endl;
std::cout << newClass.att2 << std::endl;
std::cout << newClass.att4 << std::endl;
std::cout << newClass.att5 << std::endl;
```

## 类继承

在某些情况下，我们可能希望创建多个类，但是每个类都有许多相同的属性。在这种情况下，我们可以创建一个从某个父类继承这些属性的类。例如，我们可以创建一个父类:

```
class Parent():
    def __init__(self, att1, att2):
        self.att1 = att1
        self.att2 = att2
    def print_att_1(self):
        print(att1)
    def print_att_2(self):
        print(att2)
```

然后，我们可以通过创建新类并使用父类作为属性，在新的子类中使用父类的属性；init 函数将包含从父类继承的所有属性，以及我们希望包含在子类中的任何新属性。最后，在 init 类的主体中，我们必须包含 super()方法，该方法允许我们从父类复制属性:

```
class Child(Parent):
    def __init__(self, att1, att2, att3, att4):
        super().__init__(att1, att2)
        self.att3 = att3
        self.att4 = att4
        def print_att_3(self):
            print(att3)
        def print_att_4(self):
            print(att4)
```

在 C++中，我们可以像这样创建一个父类:

```
class Parent
{
    public:
    int att1;
}
```

当我们创建子类时，我们通过定义子类和名字来指定从父类的继承，使用冒号作为分隔符，然后指定父类的访问模式和父类的名字；这使得子类可以访问父类的公共或私有访问模式主体中的所有内容。从这里开始，我们像平常一样创建类体，包括子的访问模式和我们想要包含的任何属性和方法:

```
class Child : public Parent
{
    public:
    int att2;
}
```

调用和访问子类的属性和方法与调用任何其他类是一样的。

## 字符类示例

下面是用 Python 编写的一个类，它创建了一个 RPG 角色，包括名字、种族、专长、生命值和魔法值。

Python:

```
class Character():
    """ A class that holds character information """ def __init__(self, name="", race="", specialty="", hp=0, mp=0):
        self.name = name
        self.race = race
        self.specialty = specialty
        self.hp = hp
        self.mp = mp def set_name(self):
        """ Sets character name """
        print("What is your name?")
        self.name = input() def set_race(self):
        """ Sets character race """
        print("What is your race? Choose between human, elf, or  
              dwarf.")
        self.race = input() def set_specialty(self):
        """ Sets character class/specialty """
        print("What is your specialty (or class)? Choose between    
              warrior, mage, or rogue.")
        self.specialty = input() def set_hp(self):
        if self.specialty == "Warrior":
            self.hp = 25
        elif self.specialty == "Mage":
            self.hp = 15
        elif self.specialty == "Rogue":
            self.hp = 20
        print(f"You have {self.hp} hit points.") def set_mp(self):
        if self.specialty == "Warrior":
            self.mp = 5
        elif self.specialty == "Mage":
            self.mp = 15
        elif self.specialty == "Rogue":
            self.mp = 10
            print(f"You have {self.mp} magic points.") def view_character_stats(self):
        print(f"Name: {self.name}")
        print(f"Race: {self.race}")
        print(f"Class: {self.specialty}")
        print(f"HP: {self.hp}")
        print(f"MP: {self.mp}")newChar = Character()
newChar.set_name()
newChar.set_race()
newChar.set_specialty()
newChar.set_hp()
newChar.set_mp()
newChar.view_character_stats()
```

退货:

```
$ python3 classes.py>> What is your name?
>> Daniel
>> What is your race? Choose between human, elf, or dwarf.
>> Human
>> What is your specialty (or class)? Choose between warrior, mage,  
   or rogue.
>> Warrior
>> You have 25 hit points.
>> You have 5 magic points.
>> Name: Daniel
>> Race: Human
>> Class: Warrior
>> HP: 25
>> MP: 5
```

C++:

```
#include <iostream>
#include <string>class Character
{
    public:
        std::string name;
        std::string race;
        std::string specialty;
        int hp;
        int mp; std::string setName()
        {
            std::cout << "What is your name?\n";
            std::cin >> name;
            return name;
        }; std::string setRace()
        {
            std::cout << "What is your race? Choose between human, 
                          elf, or dwarf.\n";
            std::cin >> race;
            return race;
        }; std::string setSpecialty()
        {
            std::cout << "What is your specialty (or class)? Choose  
                          between warrior, mage, or rogue.\n";
            std::cin >> specialty;
            return specialty;
        }; int setHP()
        {
            if (specialty == "Warrior")
            {
                hp = 25;
            }
            else if (specialty == "Mage")
            {
                hp = 15;
            }
            else if (specialty == "Rogue")
            {
                hp = 20;
            } std::cout << "Your hp is " << hp << std::endl;
            return hp;
        }; int setMP()
        {
            if (specialty == "Warrior")
            {
                mp = 5;
            } else if (specialty == "Mage")
            {
                mp = 15;
            }
            else if (specialty == "Rogue")
            {
                mp = 10;
            } std::cout << "Your mp is " << mp << std::endl;
            return mp;
        }; void viewCharStats()
        {
            std::cout << "Name: " << name << std::endl;
            std::cout << "Race: " << race << std::endl;
            std::cout << "Class: " << specialty << std::endl;
            std::cout << "HP: " << hp << std::endl;
            std::cout << "MP: " << mp << std::endl;
        };};int main()
{
    Character myChar; // Create new character object
    myChar.setName(); // Call setName method
    myChar.setRace(); // Call setRace method
    myChar.setSpecialty(); // Call setSpeciality method
    myChar.setHP(); // Call setHP method
    myChar.setMP(); // Call setMP method
    std::cout << myChar.name << std::endl;
    myChar.viewCharStats(); // Call viewCharStats method
    return 0;
};
```

退货:

```
$ g++ -o classes classes.cpp
$ ./classes>> What is your name?
>> Daniel
>> What is your race? Choose between human, elf, or dwarf.
>> Elf
>> What is your specialty (or class)? Choose between warrior, mage,  
   or rogue.
>> Mage
>> Your hp is 15
>> Your mp is 15
>> Daniel
>> Name: Daniel
>> Race: Elf
>> Class: Mage
>> HP: 15
>> MP: 15
```

勇敢的读者们，感谢你们再次和我一起踏上从 Python 到 C++系列的另一段旅程。我希望下次能见到你，并一如既往地保持快乐编码！