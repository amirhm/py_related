# python generators

generator functions return iterators instead of the results. (results are not calculated in the function run time, rather later when excuted)

** are optimized in terms of memory usage, but not interms of time**

exp:
```python
def func(lst):
    for n in lst:
        yield n**2
```
now if this funtion ran on a list the funtion will return a iterator. 

we could as well use multiple yield generator functions.
```python
def func(lst):
    name  = "hello"
    yield name
    name  = "world"
    yield name
```

we could as well have the genrator expressions, similar to list comprehension, just this time we only use the (). ex:

```python
l = [i for i in range(10)] # list comprehension
l = (i for i in range(10)) # generator expression 
```


# Python disassembler
``` python 
import dis
dis.dis
```
Disassemble classes, methods, functions, and other compiled objects.

#python profiler
use the importtime-waterfall to see the import time for the libraries 
```
importtime-waterfall [package] --har (to be in har format)
```

# using cProfile

this will gives us the binary results in out file.
```
python3 -m cProfile train.py -o out.pstats run
```
now we could use the pstats module to look into the out file.
```
python3 -m pstats out.pstats 
```
or use gprof2dot as much better visualization
``` 
gprof2dot out.pstats | dot -Tsvg -o log.svg
```
whish will create a digraph output 




# `__future__` module
This module really exist, see the doc by `python -m pydoc __future__`
this is for the migration compatibility, generally the feature first appear here and then finilized in the releases. consider this as a compilation flags which loads the module with different settings. 
for example if more python3 like print is needed we could add the 
```
from __future__ import print_function
```
to ensure the same functionality if loaded with python 2. 
This import should appear before any line in the code (except after the docstring), otherwise docstring is not accepted. 

# compeltion on python 

to have the auto completion on python (similar the ipyhton), we could use the rlcompleter and readline modules
```python
import rlcompleter
import readline
readline.parse_and_bind("tab: complete")
``` 

# python for...: else , while ...: else

this is quite unique fot python and else statement is excuted when the loop is not broken by either return, raise or break or so.






