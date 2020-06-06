


```python
def test(*,a) # should be keyword only argument
def test(a, /) # a should be keyword only argument
```
`*` in function arguments collect all the positional arguments (as a tuple)
`**` collect all the keyword arguments as a dictionary
if we defint a function as `test(*x)` similarly will be a positional only function, means not possible to pass the keyword arguments.

---
also * is for uppack : for all iteratable object in python (list, tuple or dictionaries)
** splat -> give a dictionary as function arguments


# Argparse examples:

``` python
import argparse
from typing import Optional
from typing import Sequence

def main(argv: Optional[Sequence[str]] = None) -> int:
    parser = argparse.ArgumentParser()
    parser.add_argument('filename',help='add help text') #positional arguments
    parser.add_argument('-c','--config','--jsonfile',default='test.json',help='help text') 
    parser.add_argument('-d','--days',type=int,default='test.json',help='help text') # for type we could pass a function too
    parser.add_argument('-v','--verbose',action='count',default=0) # -vvvv -> verbose = 4 
    # boolean 
    parser.add_argument('--force',action='store_true') 
    #append
    parser.add_argument('--logs',action='append',default =[]) # --logs test1.log --logstest2.log 
    #choices
    parser.add_argument('--color',choices=('red','green')) # --logs test1.log --logstest2.log 

    # sub-commands
    subparser = parser.add_subparsers(dest='comand')
    st_parser = subparser.add_parser('status')
    st_parser.add_subparsers(.....)

    args = parser.parse_args(argv)
    print(args.filename)
    print(vars(args))
    return 0

if __name__ == "__main__":
    exit(main())
```

another way to use the parser would be like this:
```python
def parse_args():
    parser = argparse.ArgumentParser()
    parser.add_argument(....)
    ....
    args = parser.parse_args()
    return args
 if __name__ == "__main__":
    args = parse_args()
    main(args)
```