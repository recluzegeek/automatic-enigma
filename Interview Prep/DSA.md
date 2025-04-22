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
