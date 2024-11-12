# Създаване на програми с класове и обекти . Наследяване

## Какво представляват класовете и обектите?

Python е обектно-ориентиран език. Това означава, че в него се използват класове и обекти. Най-общо казано обектите в Python представляват модели на реални обекти от действителността.(?????????) Обектите могат да включват свойства и методи. Класовете пък могат да се разглеждат като шаблони за производство на обекти.

## Как се създават и използват класове и обекти в Python?

За да създадем клас в Python използваме ключовата дума class

```python
class Myfirstclass:
  x=5
```

За да създадем обект, базиран на този клас, използваме израза :

```python
MyFirstObject = Myfristclass()
```

Или иначе казано, клас == шаблона който използваме ; обект == резултата от използването на този шаблон.

Елементите на класа достъпваме чрез използване на името на класа и името на елемента, разделени с точка:

```python
print(MyFirstObject.x)
```

Всеки клас в Python има вградена функция конструктор **__init__()** , която се изпълнява автоматично винаги, когато се създава обект на базата на класа. Тази функция може да се използва , за да задаваме стойности на свойства при инициализация или да извършваме други действия, които са необходими при създаването на обектите.

Например за класа Person може да предефинираме функцията **__init__()** , така че тя да задава стойности на полетата name и age по следния начин:

```python
class Person:
  def __init__(self , name , age):
    self.name = name
    self.age = age

MyPerson = Person('Ivan' , 35)
print(MyPerson.name)
print(MyPerson.age)
```

Освен свойства , обектите могат да съдържат и методи. *Методите на обектите са функции, които принадлежат на обектите. * Например за създадения вече клас може да създадем метод *greetings* , който отпечатва поздравление от съответното лице:

```python
class Person:
  def __init__(self , name ,age):
    self.name = name
    self.age = age
  def greetings(self):
    print("Hello , my name is  : " + str(self.name))

MyPerson = Person("Ivan" , 35)
MyPerson.greetings()
```

Параметърът **self** е референция къ, текущата инстанция на класа. Използва се за достъп до свойствата и методите, принадлежащи на класа. Не е задължително той да се нарича self. Може да го наричаме с друго име, важното е да е първия параметър на всеки метод от класа.

Можем да променяме стойности на свойства и обекти, например:

```python
MyPerso.age = 40
```

Също така, можем да изтриваме свойства на ожекти с ключовата дума del. Можем и да трием цели обекти по същия начин.

Дефиницията на клас не може да е празна, но ако по някаква причина искаме да оставим клас без съдържание, може да използваме ключовата дума **pass**.

## Какво представлява наследяването?

Наследяването е едно от 3те основни неща с които се характеризира [OOP](https://www.techelevator.com/the-3-pillars-of-object-oriented-programming-oop-brought-down-to-earth/#:~:text=There%20are%20three%20major%20pillars,encapsulation%2C%20inheritance%2C%20and%20polymorphism).

Това е подход, който позволява един клас да наследи всички методи и свойства на друг клас. Наследяваният клас се нарича клас-родител. Наследяващият клас се нарича клас-дете.

Всеки клас може да бъде родителски. Създаването на родителски клас е същото, като на всеки друг клас.
Като пример създаваме клас , наречен Person, който има свойства firstname и lastname и метод printname :

```python
class Person:
  def __init__(self , fname , lname) :
    self.firstname = fname
    self.lastname = lname
  def printname(self):
    print(self.fristname , self.lastname)
```

Може да използваме този клас , за да създадем ожект и да извикаме printname.

```python
MyPerson = Person("Ivan" , "Petrov")
MyPerson.printname()
```

За да създадем клас , който наследява даден клас, подаваме продителския клас като параметър при създаването на класа-наследник.

Например, можем да създадем клас Student , който наследява методите и класовете на класа Parent

```python
class Student(Person):
  pass
```

Тук е подадена ключовата дума pass , защото за момента не е необходимо да се добавят нови свойства или методи към този клас. Така създаден, класът-наследник има същите свойства и методи като родителския клас. За него например може да създадем обек и да извикаме метода printname

```python
MyStudent = Student("Petar" , "Vasilev")
MyStudent.printname()
```

В класа-наследник можем да предефинираме функцията __init__(). Тогаава класът-наследник вече не наследява тази функция от родителя, а има свой собствен вариант за нея. Например, за да предефинираме в класа Student функцията __init__() , така че тя да прави същото като в родителския клас , може да използваме един от двата варианта:

```python
class Student(Person):
  def __init__(self , fname , lname):
    Person.__init__(self, fname , lname)
```

или 

```python
class Student(Person):
  def __init__(self, fname , lname):
    super().__init__(self , fname , lname)
```

Като използваме функцията **super()** , даваме указание на класа-наследник автоматично да взима наследените от родителя методи и свойства.
Наследяващият клас може да има свои добавени свойства и методи. Например към класа Student може да се добави свойство graduationyear:

```python
class Student(Person):
  def __init__(self, fname , lname):
    super().__init__(self , fname , lname)
  self.graduationyear=2019
```

За да се вземе под внимание новото свойство при инициализация, може да се промени методът __init__() , така че graduationyear да се задава при инициализация.

```python
class Student(Person):
  def __init__(self, fname , lname, year):
    super().__init__(self , fname , lname)
  self.graduationyear=2019
```
Така при създаването на обекта се подава коректната година. 

## Задължителни задачи 

1. Да се напише код , който дефинира клас Person с полета име, фамилия , възраст , националност. Да се дефинира конструктор, който инициализира полетата на класа. Да се добави метод print който отпечатва имената и националността на съответното лице. Да се създадът обекти- инстанции на класа. За тях да се извика методът print

2. Към първата задача , да се добави наследник на класа с две нови полета - университет и година на обучение. Да се предефинира за него методът print, така че да отпечатва тези две полета.

3. Да се добави още един клас - Lecturer , наследник на класа Person с две нови полета - университет и опит. Да се предефинира методът print.

## Допълнителни: 

In this exercise, you will build a simple text-based RPG game using classes, objects, and inheritance. You will create different character types with unique abilities and set up a battle scenario against an enemy.

### Instructions
Create the Base Class Character:

Define attributes for name, health, attack_power, and defense.
Define the following methods:

- attack(target): Reduces the target’s health by self.attack_power.
- take_damage(amount): Reduces the character's health by amount, considering the character’s defense.
- is_alive(): Returns True if health > 0, otherwise False.

Create Subclasses for Warrior, Mage, and Archer. Each subclass should inherit from Character. Each subclass should have a unique version of the attack method:
- Warrior: Deals additional damage when health is low.

- Mage: Occasionally deals double damage by casting a "spell."

- Archer: Has a chance to land a critical hit that increases damage.

Create an Enemy Class:
Define attributes similar to the Character class (e.g., name, health, attack_power, defense).
Implement similar methods to handle the enemy’s attack and damage.

Game Loop:
Prompt the player to choose a character class (Warrior, Mage, or Archer).
Create an instance of the chosen character and an Enemy (e.g., a Goblin).
Create a simple turn-based battle loop where the player and enemy take turns attacking each other.
The game ends when either the player or the enemy's health reaches 0.

- Example Interaction
```
Copy code
Welcome to Battle of the Realms!
Enter your character's name: Thorin
Choose your class (Warrior, Mage, Archer): Warrior

A wild Goblin appears!

Do you want to (A)ttack or (R)un? A
Thorin attacks the Goblin.
Goblin takes damage! Remaining health: 20

Goblin attacks back!
Thorin takes damage! Remaining health: 45
```

