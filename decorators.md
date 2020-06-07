Decorators
==========

decorators are used, when we want to king of wrap the function inside the decorator.
Like if `test_func()` is decorated with `@dec` function , 
then test is first passed to dec function. 
Very simple example of decorators are like this:


```` python
def dec(func):
  print("dec function") #This will only printed First time
  return func

@dec 
test_func(x): 
  print(x)
````

In example above then, **test_func** is passed to the dec decorator, and returned with no changes. The only difference is that when function defined the decorator is executed and will print the "dec fucntion". 
In practice, we will pass the original function to modify, like this example:

```python
import functools


def dec(func):  
  @functools.wraps(func) #this line to keep the riginal attributes
   def inner_func(*args,**kwargs):
    ret = func(*args,**kwargs)
    return ret
  return inner_func 
```


Other more frequently used built in decorator, which are usefull in building the classes are:

## @property
Used when a property is defined based on other variables and had to be computed. But we want to represent it as a porperty. and how to implement the setter and deleter for the defined property. 
For such property to delete , ` del loc.loc`

```python
class C:
  def __init__(self,x,y):
    self.x = x
    self.y = y

  @property
  def loc(self):
    return [self.x,self.y]
  @loc.setter
  def loc(self,loc)
    self.x, self.y = loc 
  @loc.deleter
  def loc(self)
    self.x =  self.y = 0  

  @classmethod
  def func(cls):
    print(f'in class {cls}')
  @staticmethod
  def staticfunc():
    print('static function')
```

## @classmethod

other decorator is the class method which is defined as above and in this case the function act as a function on both class or instance of the class. 
```python 
C.func() #valid
C(1,2).func() #valid
```

## @staticmethod
very similar to staic function in C, here as well the function could be called on both class and instance of the class.
```python 
C.staticfunc() #valid
C(1,2).staticfunc() #valid
```

# Implementing the `__getitem__()`

and overload example, this is to mimic or equvalence of what as overload we have in other programming languages. 


```python
from typing import TypeVar
from typing import Generic
from typing import Sequence
from typing import Union
from typing import overload

T = TypeVar('T')

class C(Generic[T]):
    def __init__(self,lst:Sequence[T]) -> None: 
        self._lst = lst

    @overload 
    def __getitem__(self, idx:int) -> int : ...

    def __getitem__(self, idx:Union[slice,int]) -> Union['C[T]',int]:
        if isinstance(idx,slice):
            return C(self._lst[idx])
        else:
            return self._lst[idx]

    def __repr__(self): #this will be the representation of the class
        return f'{type(self).__name__}'

a = C([1,2,3])

print(type(a[1]))
print(type(a[1:]))
```
