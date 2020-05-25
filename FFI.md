# python FFI example

to call the c codes (build as shared lib) from python code
take example of the c code below:
``` c
void fill(int *val, int b)
{
    int i;
    for(i = 0;i<b;i++)
    {
    val[i]+=5;
    }
}
int sum(int a, int b)
{
    return (a+b);
}
```
if the code is in cpp then put the declaration on the extern "C" { }.

we could build this as follow into libt.so library
```` bash
gcc -Wall -shared -fPIC -o libt.so test_c.c
````

Now from python it is easy to call the functions defined:
```` ipython
In [1]: import cffi

In [2]: ffi = cffi.FFI()
In [3]: ffi.cdef(""" int sum( int a,int b);  """)
In [4]: ffi.cdef(""" void fill (int *,int b); """)

---- OR ----
In [3]: ffi.cdef(""" int sum( int a,int b); void fill (int *,int b); """)


In [5]: m = ffi.dlopen('libt.so')
In [6]: m.sum(2,3)
Out[6]: 5

In [7]: a = ffi.new("int [10]")
In [8]: m.fill(a,10)

In [25]: a[0]
Out[25]: 5

In [29]: (a+1)[5]
Out[29]: 5
````
To get a pointer from a numpy array
````python
In [73]: a = np.random.randn(100,2)
In [74]: a[0]
Out[74]: array([-0.69150089, -0.26278219])
In [75]: b = ffi.cast("double *",a.ctypes.data)
In [76]: b[1]
Out[76]: -0.26278218554490773
````
similar for pytorh tensors:

````python 
a = torch.DoubleTensor(10,2)
b = ffi.cast("double * ",a.data_ptr())
````


