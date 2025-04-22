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
