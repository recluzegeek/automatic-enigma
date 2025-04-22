# Python as Language of Choice

## Python Refresher

```python
x = 5 # create variable x with value 5

# function definition
def good_enough(x, guess):
    return abs((guess * guess) - x) < 0.0000000001

def improve_guess(x, guess):
    return (guess + x/guess)/2

# optional params => method overloading
def sqrt(x, guess=0.0001):
    if good_enough(x, guess):
        return guess
    else:
        return sqrt(x, improve_guess(x, guess))

print(sqrt(36)) # Function calling

x = 17

if x < 5:
    print("x is less than 5")
elif x < 7:
    print("x is less than 10")
else: 
    print("x is greater than 7")

# loops
p = 0
while(p < 10):
    print(p)
    p+=1

for i in range(10):
    print(i)

# lists
l = [90, 100, 21, 39]
for i in l:
    print(i)

# can be accessed via index
l[100] # returns 1

# dictionaries
d = {
    "id" : 21237,
    "name" : "recluzegeek",
    "languages_spoke": 3
}

print(d)
print(d.keys()) # return keys of the dict
print(d.values()) # return values
print(d.items()) # return keys and values

for k, v in d.items():
    print(k," => ", v)

# tuples
print(tuple(d.items()))# casting, list => tuple
t = tuple(1, 2, 4, 8) # immutable once created

a = ["id", "name", "city"]
b = [3245, "John", "Davis"]
merged = zip(a, b)

print(merged) # Prints the memory location of the iterator, not the merged list
print(list(merged))  # Now it will print the merged pairs in a list: [('id', 3245), ('name', 'John'), ('city', 'Davis')]
dict(zip(a, b)) # map indices of both lists, create dict for storing in database

print(range(0, 100)) # Prints the memory location of the range object
print(list(range(0, 100))) # Creates a list of numbers from 0 to 99 and prints them

# In other words, when using zip or range, you're working with a lazy evaluation mechanism, 
# where the data is generated and consumed as you need it. This makes them more memory-
# efficient than creating and storing large data structures upfront.
```

## OOP Concepts in Python

Creating a class in python is simple with minor difference of using `self`. To access variables and methods of the class, we need to use `self`, instead of `this` in python.

```python
class Point():

    # special functions, starts and ends with double underscore __, also 
    # known as dunder (double + underscore) functions. They're called
    # automatically by python, when needed like __init__ called upon,
    # when object needs to be created, __str__ is called when we're required the 
    # string representation of the object

    # In python, method overloading is not supported. So 

    # overloaded constructor
    def __init__(self, x = 0, y = 0):   # constructor function, notice the first argument of self
        self.x = x  # using 'self', instead of 'this' to refer the instance variable x
        self.y = y

    # special function to return string representation of the object
    def __str__(self): # every class method, needs to have self as first argument
        
        # explicit over implicit to avoid ambiguity for referring to instance variable x
        return "[" + str(self.x) + ", "+ str(self.y)+ "]"

p1 = Point() # p1 is the reference variable
print(p1.x)

p2 = Point(4, 3)
p2 # returns memory address of the p2 object
print(p2) # prints [4, 3] because python called __str__ internally automatically
```

### Composition in Python

In Python, we can dynamically add methods to a class at runtime, similar to how "friend functions" are handled in C++, but with the full power of class methods. This flexibility allows us to define methods even after a class has already been defined.

```python
class Shape():

    def __init__(self, points):
        self.points = points

    # def __str__(self):
    #     return " -- ".join(str(point) for point in self.points)

p1 = Point(5, 5)
p2 = Point(10, 5)
p3 = Point(5, 10)
p = [p1, p2, p3] # right angle triangle

s = Shape(p)
print(s)

# we can add methods to an already defined class as well
# similar to JS, where functions can be set as instance variable 

def display_points(self):
    return "..".join(str(point) for point in self.points)

# adding display_point function to Shape class as display method dynamically
Shape.display = display_points
s.display()
```

### Inheritance in Python

Inheritance in python is quite easy to do. Just add the class name in the `()`, you want to inherit from.

```python
# Triangle class inheriting from Shape class
class Triangle(Shape):
    pass # pass means, I'm not going to write anything in this block, in future maybe i'll write

t = Triangle(p)
t.display() # calling function from parent class of Shapes

# also we can add dynamically functions in the child class as well
```

#### Access Parent Class's Overridden Methods

We can use the `super` keyword to access and invoke the implementation of a parent class's method that has been overridden in a subclass.

```python
class Rectangle():

    def __init__(self, length, width):
        self.length = length
        self.width = width

    def get_area(self):
        return self.length * self.width

    def perimeter(self):
        return 2 * (self.length + self.width)

    def __str__(self):
        return "[ L: " + str(self.length) + ", W: " + str(self.width) + "]"

r = Rectangle(3, 4)
r.get_area() # 12

# class Square is extending Rectangle class
class Square(Rectangle):

    def __init__(self, length):
        # calling parent class constructor via super()
        super().__init__(length, length)

    # accessing the parent __str__ inside child __str__ with super()
    def __str__(self):
        return "Square: " + super().__str__()

s = Square(3)
print(s.get_area()) # 9
print(s) # Square: [L: 3, W: 3]
```

### **Encapsulation in Python**

Encapsulation is about hiding the internal state and functionality of an object. It is typically achieved using private and protected attributes (denoted by underscores).

- **Single underscore (_)**: This is a convention indicating that a variable or method is intended to be protected and should not be accessed directly outside the class. However, it’s not enforced by Python.
  
- **Double underscore (__)**: This triggers name mangling, making the variable harder to access from outside the class, although it’s still possible to access it with the mangled name.

```python
class MyClass:
    def __init__(self):
        self._protected = "Protected Value"
        self.__private = "Private Value"

    def get_private(self):
        return self.__private

obj = MyClass()
print(obj._protected)   # Allowed (but discouraged)
print(obj.__private)    # AttributeError: 'MyClass' object has no attribute '__private'
print(obj.get_private())  # Correct way to access private attribute
```

### **Difference Between Interface and Abstract Class**

| **Concept**          | **Interface**                                           | **Abstract Class**                                       |
|----------------------|---------------------------------------------------------|---------------------------------------------------------|
| **Purpose**          | Defines a contract for what methods a class should implement. | Defines a base class with common functionality, can have both abstract and concrete methods. |
| **Methods**          | All methods are abstract (no implementation).          | Can have both abstract methods and concrete methods (with implementation). |
| **Multiple Inheritance** | A class can implement multiple interfaces.          | A class can inherit from only one abstract class.        |
| **Usage**            | Used to define common behavior that can be implemented differently by subclasses. | Used when you have shared functionality that you want to be inherited by subclasses. |

### **Difference Between Abstraction and Encapsulation**

| **Concept**          | **Abstraction**                                              | **Encapsulation**                                           |
|----------------------|--------------------------------------------------------------|-------------------------------------------------------------|
| **Definition**       | Hiding complexity and exposing only essential features.     | Hiding the internal details of an object, restricting access. |
| **Goal**             | Focuses on **what** an object does (the interface).         | Focuses on **how** data is stored and protected.             |
| **Visibility**       | Abstract classes and methods cannot be instantiated directly. | Data and methods are encapsulated (via access modifiers).   |
| **Example**          | Abstract classes and methods.                               | Private variables and methods (using `_` or `__`).           |
