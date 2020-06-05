Python debugger
===============
pdb could be use in two mode, either just like ussual debugger to set break point and debug the code or in post-mortem mode when crash has happened.

For normal debug mode insert the line to stop the debugger at the line needed
```` python
import pdb ; pdb.set_trace()
````
now if the file is runed debugger will be stoped at this line. ? for help, c for continue , n : next line , s : step in , b [line number] to set the break point. to see the full list hit ?.
if the trace has not been placed on the code still it is possible to run the code by pdb where in this case the debugger will be enabled ans stay at first line of the code.
````bash
python -m pdb file_to_be_debugged.py
````

to run the post-mortem mode either just run the file by debugger or in some scenarios where the error stacks are not returned after crash (like when it catched by try except) we can invoke the debugger there. 
``` python
def real_main():
    # some code here which is buggy
    pass 

def main():
    try :
        return real_main()
    except Exceptions as e:
        import pdb; pdb.post_mortem()
        print(f"error happend {type(e).__name__}: {e}")
        return 1

if __name__ == "__main__":
    exit(main())
```

The other way is to sue the `--pdb` option when the pytest is used, in that case if the test failed then it retunes a pdb terminal.
example
```python
def test_funca():
    value = funca([1,2,3,4])
    assert value == 1
```
```bash
pytest testfile.py --pdb
```

