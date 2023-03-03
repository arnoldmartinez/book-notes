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

#### Infinity

There is a special value in JavaScript called *Infinity*. It represents a number too big for JavaScript to handle. *Infinity* is indeed a number, as typing *typeof Infinity* in the console will confirm. The biggest number JavaScript can handle is 1.7976931348623157e+308, while the smallest is 5e-324.

Infinity;
**Infinity**

typeof Inifinity;
**number**

1e309;
**Infinity**

Dividing by zero gives you infinity:

var a = 6 / 0;
a;
**Inifinity**

How about the smallest? It's infinity with a minus sign in front of it; minus infinity:

var i = -Infinity;
i;
**Infinity**

typeof i;
**"number"**

Does this mean you can have something that's exactly twice as big as *Infinity*, from 0 up to infinity and then from 0 down to minus infinity? Well, not really. When you sum infinity and minus infinity, you don't get 0, but something that is called *NaN* (Not a Number):

Infinity - Infinity;
**NaN**

-Infinity + Infinity;
**NaN**

Any other arithmetic operation with *Infinity* as one of the operands gives you *Infinity*:

Infinity - 20;
**Infinity**

-Infinity * 3;
**-Infinity**

Infinity / 2;
**Infinity**

Infinity - 99999999999999999;
**Infinity**

#### NaN

It turns out that despite its name, "Not a Number", *NaN* is a special value that is aldo a number:

typeof NaN
**"number"**

var a = NaN
a;
**NaN**

Your get *NaN* when you try to perform an operation that assumes numbers, but the operation fails. For example, if you try to multiply 10 by the character "f", the result is NaN, because "f" is obviously not a valid operand for a multiplication:

var a = 10 * "f";
a;
**NaN**

*NaN* is contagious, so if you have even one *NaN* in your arithmetic operation, the whole result goes down the drain:

1 + 2 + NaN;
**NaN**

#### Strings

In JavaScript, any value placed between single or double quotes is considered a string. This means that 1 is a number, but "1" is a string. When used with strings, *typeof* returns the string "string"

var s = "some characters";
typeof s;
**"string"**

#### String conversions

When you use a number-like string(for example "1") as an operand in an arithmetic operation, the string is converted to a number behind the scenes. This works for all arithmetic operations except addition, because of its ambiguity:

var s = '1';
s = 3 * s;
typeof s;
**"number"**

A lazy way to convert any number-like string to a number is to multiply it by 1 (another way is to use a function called *parseInt()*, as you'll see in the next chapter):

var s = "100"; typeof s;
**"string"**

s = s * 1;
**100**

typeof s;
**"number"**

If the conversion fails, you'll get *NaN*:

var movie = '101 dalmatians';
movie * 1;
**NaN**

You convert a string to a number by multiplying by 1. The opposite - converting anything to a string - can be done by concatenating it with an empty string:

var n = 1;
typeof n;
**"number"**

n = "" + n;
"1"

typeof n;
**"string"**

#### Special strings

There are also strings with special meanings, as listed in the following table:

| String | Meaning                         | Example                          |
|--------|-------------------------------- |----------------------------------|
| \ \     | \ is the scape character.       | var s = "1\ \2";                |
| \ '     | Simple quotes inside the string | var s = 'I don\ 't know';       |
| \ "     | Quotes inside the string        | var s = "\ "Hello\ ", he said." |
| \n     | End of line                     | var s = '\n1\n2';                |
| \r     | Carriage return                 | var = '1\r2';                    |
| \t     | Tab                             | var s = "1\t2;"                  |
| \u     | \u allows you to use Unicode    | "\u421"                          |

There are also additional characters that are rarely used: \b(backspace), \v(vertical tab), and \f(form feed).

#### Boolean

There are only two values that belong to the Boolean data type:

* true
* false

var b = true;
typeof b;
**"boolean"**

var b = false;
typeof b;
**"boolean"**

#### Logical operators

There are three operators, called logical operators, that work with Boolean values. These are:

* ! - logical NOT(negation)
* && - logical AND
* || - logical OR

When something is not true, it must be false. Here's how this is expressed using the logical ! operator:

var b = !true;
b;
**false**

If you use the logical NOT twice, you get the original value:

var b = !!true;
b;
**true**

If you use a logical operator on a non-Boolean value, the value is converted to Boolean behind the scenes:

var b = "one";
!b;
**false**

You can convert any value to its Boolean equivalent using a double negation. Understanding how any value converts to a Boolean is important. Most values convert to *true* with the exception of the following, which convert to *false*:

* The empty string  ""
* *null*
* *undefined*
* The number 0
* The number *NaN*
* The Boolean *false*

These sis values are referred to as falsy, while all others are truthy(including, for example, the strings "0", " ", and "false").

Let's see some examples of the other two operators - the logical AND(&&) and the logical OR(||). When you use &&, the result is *true* only if all of the operands are *true*. When you use ||, the result is *true* if at least on of the operands is *true*:

var b1 = true, b2 = false;
b1 || b2;
**true**

b1 && b2;
**false**

Here's a list of the possible operations and their results:

| Operation      | Result |
|----------------|--------|
| true && true   | true   |
| true && false  | false  |
| false && true  | false  |
| false && false | false  |
| true OR true   | true   |
| true OR false  | true   |
| false OR true  | true   |
| false OR false | false  |

You can use several logical operations one after the other:

true && true && false && true;
**false**

#### Operator precedence

As you know from mathematics:

1 + 2 * 3;

**7**

This is because multiplication has higher precedence over addition, so 2 * 3 is evaluated first, as if you typed:

1 + (2 * 3);

**7**

Similarly for logical operations, *!* has the highest precedence and is executed first, assuming there are no parentheses that demand otherwise. Then, in the order of precedence, comes && and finaly ||. 

false && false || true && true

**true**

(false && false) || (true && true)

**true**

#### Lazy evaluation

If you have several logical operations one after the other, but the result becomes clear at some point before the end, the final operations will not be performed because they don't affect the end result. Consider this:

> true || false || true || false || true;

**true**


Since these are all OR operations and have the same precedence, the result will be **true** if at least one of the operands is **true**. JavaScript engine decides to be lazy (efficient) and avoids unnecessary work by evaluating code that doesn't affect the end result.

If JavaScript encounters a non-Boolean expression as an operand in a logical operation, the non-Boolean is returned as a result:

> true || "something";

**true**

> true && "something";

**"shomething"**


> true && "something" && true;

**true**

#### Comparison

There's another set of operator that all return a Boolean value as a result of the operation. These are the comparison operators. The following table lists them together with example uses:

| Operator symbol | Result                                                                | Example                |
|-----------------|-----------------------------------------------------------------------|------------------------|
| ==              | Equality comparison: Return true when both operands are equal. The operands are converted to the same type before being compared. Also called loose comparison                                                                   | > 1 == 1; **true**     |
| ===             | Equality and type comparison: Return **true** if both operands are equal and of the same type. It's better and safer to compare this way because there's no behind-the-scenes type conversions. It is called stric comparison. | > 1 === '1'; **false** |
| !=              | Non-equality comparison: Return **true** if the operands are not equal to each other (after a type conversion). | > 1 != 1; **false** |
| !==             | Non-equality comparison without type conversion: Returns **true** if the operands are not equals or if they are of different types. | > 1 !== 1; **false**   |
| >               | Return **true** if the left operand is greater than the right one.    | > 1 > 1; **false**     |
| >=              | Returns **true** if the left operand is greater than or equal to the right one. | > 1 >= 1; **true**     |
| <               | Return **true** if the left operand is less than the right one.       | > 1 < 1; **false**     |
| <=              | Return **true** if the left operand is less than or equal to the right one. | > 1 <= 1; **true**     |
                                                                                          
Note that **NaN** is not equal to anything, not even itself:

> NaN == NaN;

**false**

#### Undefined and null

If you try to use a non-existng variable, you'll get an error:

> foo;
 
 **ReferenceError: foo is not defined**
 
Using the typeof operator on a non-existing variable is not an error. You get the string "undefined" back:

> typeof foo;

**"undefined"**

If you declare a variable without giving it a value, this is, of course, not an error. But the typeof still returns "undefined":

> var somevar;
> somevar;
> typeof somevar;

**"undefined"**

This is because when you declare a variable without initializing it, JavaScript automatically intializes it with the value **undefined**:

> var somevar;
> somevar === undefined;

**true**

The *null* value, on the other hand, is not assigned by JavaScript behind the scenes;
it's assigned by your code:

> var somevar = null;

**null**

> typeof somevar;

**"object"**

Although the difference between *null* and *undefined* is small, it could be critical at times. For example, if you attempt an arithmetic operation, you get different results:

> var i = 1 + undefined;
> i;

**NaN**

> var i = 1 + null;
> i;

**1**

This is because of the different ways *null* and *undefined* are converted to the other primitive types.

- Conversion to a number:

> 1 * undefined;

**NaN**

> 1 * null;

**0**

- Conversion to a Boolean:

> !!undefined;

**false**

> !!null;

**false**

- Conversion to a string:

> "value: " + null;

**"value: null"**

> "value: " + undefined;

**value: undefined"


