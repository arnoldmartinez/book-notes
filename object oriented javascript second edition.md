# Object Oriented JavaScript
## Second Edition
### Learn everything you need to know about OOJS in this comprenhensive guide

### Object Oriented Programming
#### Objects

A object is a representation of a "thing"(someone or something) - a real-life object, or more convoluted concept. Taking a common object like a cat for example, you can see that it has certain characteristics(color, name, weight, and so on) and can perform some action(meow, sleep, hide, escape, and son on). The characteristics of the object are called *properties* in OOP-speak, and the actions are called *methods*.

There is also an analogy with the spoken language:

* Objects are most often named using nouns(books, person, and son on)
* Methods are verbs(read, run, and son on)
* Values of the properties are adjectives

#### Classes

Similar objects can be grouped based on some criteria. A hummingbird and an eagle are both birds, so they can be classified as belonging to some made up **Birds** class. In OOP, a class is a blueprint, or a recipe for an object. Another name for "object" is "instance", so we say that the eagle is one concrete instance of the general class **Birds**. You can create different objects using the same class, because a class is just a template, while the objects are concrete instance based on the template.

There's a difference between JavaScript and the "classic" OO languages such as C++, java or whatever OOP language. You should be aware right from the start that in JavaScript, there are no classes; everything is based on objects. JavaScript has the notion of prototypes, which are also objects. 
In a classic OO language, you'd say something like "create me a new object called Bob, which is of *class* Person". In a prototypal OO language, you'd say, "I'm going to take this *object* called Bob's dad that I have lying around and reuse it as a *prototype* for a new object that I'll call Bob".

#### Encapsulation

Illustrates the fact that an object contains(encapsulates) both:

* Data(stored in properties)
* The means to do something with the data(using methods)

In OOP when your code uses an object by calling its methods. It doesn't matter if you coded the object yourself or it came from some third-party library; your code doesn't need to know how the methods worki internally. 

In JavaScript, because it's an interpreted language, you can see the source code, but the concept is still the same - you work with the object's interface, without worrying about its implementation.

In JavaScript, all methods and properties are *public*, but we'll see that there are ways to protect the data inside an object and achieve privacy.

#### Aggregation

Combining several objects into a new one is known as **aggregation** or **composition**. 

Take, for example, a personal computer. It's a complex object. You cannot think about all the things that need to happen when you start your computer. But, you can abstract the problem saying that you need to initialize all the separate objects that your *Computer* object consists of - the *Monitor* object, the *Mouse* object, the *Keyboard* object, and so on. Then, you can dive deeper into each of the sub-objects. This way, you're composing complex objects by assembling resusable parts.

To use another analogy, a *Book* object can contain(aggregate) one or more *Author* objects, a *Publisher* object, several *Chapter* objects, a *TOC*(Table of Contents), and so on.

#### Inheritance

Is a elegant way to reuse existing code. For example, you can have a generic object, *Person*, which has properties such as *name* and *date_of_birth*, and which also implements the functionality *walk*, *talk*, *sleep*, and *eat*. Then, you figure out that you need another object called *Programmer*. You could re-implement all the methods and properties that *Person*, and save yourself some work. The *Programmer* object only needs to implement more-specific functionality, such as the *WriteCode* method. while reusing all of the *Person* object's functionality.

In classical OOP, classes inherit from other classes, but in JavaScript, since there are no classes, objects inherit from other objects.

When an object inherits from another object, it usually adds new methods to the inherited ones, thus extending the old object. Often the following phrases can be used interchangeably: "B inherits from A" and "B extends A". Also, the object that inherits can pick one or more methods and redefine them, customizing them for its own needs. This way, the interface stays the same, the method name is the same, but when called on the new object, the method behaves differently. This way of redefining how an inherited method works is known as **overriding**.

#### Polymorphism

In the preceding example, a *Programmer* object inherited all of the methods of the parent *Person* object. This means that both objects provide a *talk* method, among others. Now imagine that somewhere in your code there's a variable called *Bob*, and it just so happens that you don't know if *Bob* is a *Person* object or a *Programmer* object. You can still call the *talk* method on the *Bob* object and the code will work. This ability to call the same method on different objects and have each of them respond in their own way is called **polimorphism**

### Primitive Data Types, Arrays, Loops, and Conditions
#### Variables

Using a variable requires two steps. You need to:

* Declare the variable
* Initialize it, that is, give it a value

To declare a variable, you use the *var* statement, like this:

var a;
var thisIsAVariable;
var _and_this_too;
var mix12three;

For the names of the variables, you can use any combination of letters, numbers, the underscore character, and the dollar sign. However, you can't start with a number, which means that this is invalid:

var 2three4five;

To initialize a variable means to give it a value for the first time. You have two way to do so:

* Declare the variable first, then initialize it
* Declare and initialize it with a single statement

You can declare (and optionally initialize) several variables with a single *var* statement; just separate the declarations with a comma:

var v1, v2, v3 = 'hello', v4 = 4, v5;

#### Variables are case sensitive

Variables names are case sensitive. You can easily verify this statement using your JavaScript console. Try typing this, pressing *Enter* after each line:

var case_matters = 'lower';

var CASE_MATTERS = 'upper';

case_matters;

CASE_MATTERS;

#### Operators

Operators take one or two values (or variables), perform an operation, and return a value.

> 1 + 2;
**3**

In the code:

* **+** is the operator
* The operation is addition
* The input values are 1 and 2 (the input values are also called operands)
* The result value is 3
* The whole thing is called an expression

| Operator symbol | Operation                           | Example     |
|-----------------|-------------------------------------|-------------|
| +               | Addition                            | 1 + 2       |
| -               | Substraction                        | 99.99 - 11  |
| *               | Multiplication                      | 2 * 3       |
| /               | Division                            | 6 / 4       |
| %               | Modulo, the remainder of a division | 6 % 3       |
| ++              | Increment value by 1                | var b = a++ |
| --              | Decrement a value by 1              | var b = a-- |

var a = 1; is algo a operation; it's the simple assignment operation, and =  is the **simple assignment operator**.

#### Primitive data types

Any value that you use is of a certain type. In JavaScript, there are just a few primitive data types:

* **Number**. This includes floating point numbers as well as integers. For example, these values are all numbers: 1, 100, 3.14
* **String**: These consist of any number of charactes, for example "a", "one", and "one 2 three".
* **Boolean**: This can be either *true* or *false*.
* **Undefined**: When you try to access a variable that doesn't exist, you get the special value undefined. The same happens when you declare a variable withouth assigning a value to it yet. JavaScript initializes the variable behind the scenes with the value *undefined*. The undefined data type can only have one value - the special value *undefined*.
* **Null**: This is another special data type that can have only one value, namely the *null* value. It means no value, an empty value, or nothing. The difference with *undefined* is that if a variable has a value *null*, it's still defined, it just so happens that its value is nothing. You'll see some examples shortly.

Any value that doesn't belong to one of the five primitive types listed here is an object. Even *null* is considered an object, which is a little awkward - having an object that is actually nothing.

#### Finding out the value type - the typeof operator

If you want to know the type of a variable or a value, you use the special *typeof* operator. This operator returns a string that represents the data type. The return values of using *typeof* are one of the following:

* "numbers"
* *string"
* "boolean"
* "undefined*
* "object"
* "function"

#### Numbers

The simplest number is an integer, If you assign 1 to a variable and then use the *typeof* operator, it returns the string "number":

var n = 1;
typeof n;
**"number"**

#### Octal and hexadecimal numbers

When a number starts with a 0, it's considered an octal number. For example, the octal *0377* is the decimal *255*:

var n3 = 0377;
typeof n3;
**"number"**

n3;
**255**

While you may not be intimately familiar with octal numbers, you've probably used hexadecimal values to define colors in CSS stylesheets.

In CSS, you have several options to define a color, two of them being:

* Using decimal values to specify the amount of R (red), G (green), and B (blue) ranging from 0 to 255.
* Using hexadecimals and specifying two characters for each R, G, and B value.

In JavaScript, you put *0x* before a hexadecimal value

var n4 = 0x00;
typeof n4;
**"number"**

n4;
**0**

#### Exponent literals

1e1 (also written as *1e+1* or *1E1* or *1E+1*) represents the number one with one zero after it, or in other words, 10. Similarly, *2e+3* means the number 2 with 3 zeros after it, or 2000:

1e1;
**10**
