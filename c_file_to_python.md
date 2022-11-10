Usually cython is the way to go for converting the c code to python module. 

to build a shared lib (pyhton module directly from the c code, the process of building from command line could be summerized as folloe


consider modulename = t_cython

1. cython t_cython.pyx : to generate the c code 
2. use the gcc to compile to python module. 
3. On linux: (extension so, module name should be same as the file)
    ```
    gcc -shared -pthread -fPIC -fwrapv -O2 -Wall -fno-strict-aliasing \
    -I/usr/include/python3.8 -I/home/amirhm/.local/lib/python3.8/site-packages/numpy/core/include\
    -o t_cython.so t_cython.c other_c_sources.c
    ```
4. On Windows: (extension pyd)
    ```
    gcc -shared -pthread -fPIC -fwrapv -O2 -Wall -fno-strict-aliasing -IC:\Programs\Python\Python38\include\
    -IC:\Programs\Python\Python38\Lib\site-packages\numpy\core\include\
    -DMS_WIN64 -o t_cython.pyd t_cython.c other_c_sources.c  -LC:\Programs\Python\Python38\libs -lpython38
    ```
    
 example .pyx file to link with c funciton:
 
```python
cimport cython
import cython
cimport numpy as np
from libc.stdint cimport uint16_t, int16_t


cdef extern from "main.h":

    void filter(const np.int16_t* input, np.int16_t* out, np.uint16_t m, int d);


def f(np.ndarray[np.int16_t, ndim=1, mode="c"] input, np.ndarray[np.int16_t, ndim=1, mode="c"] out, int m, int d):
    filter(<const int16_t*> np.PyArray_DATA(input), <int16_t*> np.PyArray_DATA(out),  m,  d)
    return 0
``` 
