
# Iterators & Generators In Python

**Iterating Through List and Dictionary**

In Python programming language, an iterator is an object which implements the iterator protocol. The iterator protocol consists of two methods. The __iter__() method, which must return the iterator object and the next() method, which returns the next element from a sequence.

Python has several built-in objects, which implement the iterator protocol. For example lists, tuples, strings, dictionaries or files.

In general, a generator is a special routine that can be used to control the iteration behaviour of a loop. A generator is similar to a function returning an array. A generator has parameters, it can be called and it generates a sequence of numbers. But unlike functions, which return a whole array, a generator yields one value at a time. This requires less memory. (Wikipedia)

Generators in Python:

Are defined with the def keyword
Use the yield keyword
May use several yield keywords
Return an iterator



```python
myList = [1,2,4,5,'a',2.3,'PV']
for val in myList:
    print val,
```

    1 2 4 5 a 2.3 PV
    


```python
myDict = {"Bangalore":"Karnataka","Hyderabad":"Telengana","Cochin":"Kerala"}
# Here iterator is returning only keys and also note that Keys are not in order of inserion
# This should be because of the way dictionary is implemented (may be using some hash code)
for val in myDict:
    print val,
print "\n"
# .items() returns an iterator with 
for (k,v) in myDict.items():
    print k,v
    
#Create a List out of Dictionary
myList = list(myDict.items())
print myList
```

    Cochin Hyderabad Bangalore 
    
    Cochin Kerala
    Hyderabad Telengana
    Bangalore Karnataka
    [('Cochin', 'Kerala'), ('Hyderabad', 'Telengana'), ('Bangalore', 'Karnataka')]
    

** Enumerate a List and Dictionary **

Note: If we try to iterate beyond the List Stop Iteration Exception is Thrown


```python
myList = ["Cat","dog","monkey"]
myListEnum = enumerate(myList)
print myListEnum.next()
print myListEnum.next()
print myListEnum.next()
try:
    print myListEnum.next()
except StopIteration:
    print "StopIteration Exception Received"
    pass

myDictEnum = enumerate(myDict)
print myDictEnum.next()
print myDictEnum.next()
print myDictEnum.next()
```

    (0, 'Cat')
    (1, 'dog')
    (2, 'monkey')
    StopIteration Exception Received
    (0, 'Cochin')
    (1, 'Hyderabad')
    (2, 'Bangalore')
    

** LAMBDA Functions **

Lambda Functions are a way to create small anonymous functions.
Ex: Let us say we want to convert a list indicating Kmph to Mph with 1Kmph = 0.621Mph.
We can Use Lambda and map provided by Python to achieve the functionality

** Example : Using Map and Lambda to Convert Speed from Kmph to Mph **


```python
SpeedInKmph = [80,56,45,32,89,100]
SpeedInMph = map(lambda x:0.621*x, SpeedInKmph)
print SpeedInMph

#Reconvert Back to Kmph
Speed = map(lambda x: x/0.621, SpeedInMph)
print Speed

#Except for the Floating point point differences we get back the original data
```

    [49.68, 34.775999999999996, 27.945, 19.872, 55.269, 62.1]
    [80.0, 55.99999999999999, 45.0, 32.0, 89.0, 100.0]
    

** Example : Using Filter to Filter only odd numbers from a list**


```python
integers = [1,2,3,4,5,6,7,8,9]
oddNumbers = filter(lambda x:x%2,integers)
#Note in python 3 filter returns an iterator while in 2.x it returns the complete iterable

```

** Example : Using Lambda and reduce together to sum all elements in a list **


```python
listSum = reduce(lambda x,y:x+y,integers)
print listSum
```

    45
    

** Generators **

Generators are functions that return an iterator object.
Generators provide a neat way of producing data which is infinite.
Important feature of Generators is local variables and execution start is automatically saved between calls.

**Example : generator for Fibonnaci series**


```python
def fibonacci():
    a,b=0,1
    while True:
        yield a
        a,b =b,a+b

f = fibonacci() # This is the generator
print f.next(),f.next(),f.next(),f.next()

```

    0 1 1 2
    

# Writing own iterators

Iterators contain two methods __iter__ and next. By Providing these methods we can make iterators/Generators.
iter provided by python calls __iter__ method of the argument/object provided to it

-Example : xrange provided by python can be implemented in below manner


```python
class yrange:
    def __init__(self,n):
        self.i=0
        self.n=n
        
    def __iter__(self):
        return self
    
    def next(self):
        if self.i < self.n:
            i=self.i
            self.i = self.i+1
            return i
        else:
            raise StopIteration()
            
y=yrange(5)
print list(y)
```

    [0, 1, 2, 3, 4]
    

** Question : Write definition for reverse_iter . **



```python
class reverse_iter:
    def __init__(self,l):
        self.l =l
        self.i = len(l)
    def __iter__(self):
        return self
    def next(self):
        if self.i <= 0:
            raise StopIteration()
        else:
            i = self.i
            self.i = self.i-1
            return self.l[i-1]
        
l = [1,2,3,4,5]
print list(reverse_iter(l))
```

    [5, 4, 3, 2, 1]
    

** Generator Expressions **

Generator expressions are like list comprehensions except that they return a generator than an iterator.



```python
gen = (x*x for x in xrange(10)) #Note here how parantheses are used wheras List comprehension uses square brckets
print gen
print gen.next(),gen.next()
print sum(gen)  # Note here as '0' and '1' have already been accessed using .next() expected sum is less by 1. i,e 284 instead of 285
```

    <generator object <genexpr> at 0x0000000003E83C60>
    0 1
    284
    

** Question : Generate First 10 Pythogerean Triplets  **
    
    (x,y,z) such that x*x+y*y = z*z


```python
def integers():
    i = 0
    while True:
        yield i
        i = i+1
        
triplets= ((x,y,z) for z in integers() for y in xrange(1,z) for x in range(1,y) if x*x+y*y == z*z  )

def take(n,generator):
    result = []
    for i in xrange(10):
        result.append(generator.next())
    return result
take(10,triplets)
```




    [(3, 4, 5),
     (6, 8, 10),
     (5, 12, 13),
     (9, 12, 15),
     (8, 15, 17),
     (12, 16, 20),
     (15, 20, 25),
     (7, 24, 25),
     (10, 24, 26),
     (20, 21, 29)]



 Write a program that takes one or more filenames as arguments and prints all the lines which are longer than 40 characters.
 


```python
def Print40(filenames):
    for file in filenames:
        for line in file:
            if len(line) > 40:
                print line
```

# Using itertools - Standard Module available for general purpose iterators



```python
# itertools combinations can be used to genarate combinations of a list
import itertools
print list(itertools.combinations(range(5),2))
print list(itertools.combinations(range(5),3))
print "\n"
print list(itertools.combinations(list(iter("Prakash")),5))

```

    [(0, 1), (0, 2), (0, 3), (0, 4), (1, 2), (1, 3), (1, 4), (2, 3), (2, 4), (3, 4)]
    [(0, 1, 2), (0, 1, 3), (0, 1, 4), (0, 2, 3), (0, 2, 4), (0, 3, 4), (1, 2, 3), (1, 2, 4), (1, 3, 4), (2, 3, 4)]
    
    
    [('P', 'r', 'a', 'k', 'a'), ('P', 'r', 'a', 'k', 's'), ('P', 'r', 'a', 'k', 'h'), ('P', 'r', 'a', 'a', 's'), ('P', 'r', 'a', 'a', 'h'), ('P', 'r', 'a', 's', 'h'), ('P', 'r', 'k', 'a', 's'), ('P', 'r', 'k', 'a', 'h'), ('P', 'r', 'k', 's', 'h'), ('P', 'r', 'a', 's', 'h'), ('P', 'a', 'k', 'a', 's'), ('P', 'a', 'k', 'a', 'h'), ('P', 'a', 'k', 's', 'h'), ('P', 'a', 'a', 's', 'h'), ('P', 'k', 'a', 's', 'h'), ('r', 'a', 'k', 'a', 's'), ('r', 'a', 'k', 'a', 'h'), ('r', 'a', 'k', 's', 'h'), ('r', 'a', 'a', 's', 'h'), ('r', 'k', 'a', 's', 'h'), ('a', 'k', 'a', 's', 'h')]
    
