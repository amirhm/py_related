Ctypes examples in python
=========================

This is another ways than using the cffi to load and use the precompiled libraries from python.
ctypes package should be imported on the python and after that basically just by load ing the libraries you could simply use 

for the baisc c functionalities of io manipulations the relative libraries should be loaded:

for windows:  msvcrt.dll
for mac: libSystem.dylib
for linux: libc.so
````python
libc = ctypes.CDLL("libSystem.dylib")
In [3]: fclose = libc.fclose 
  ...: fclose.argtypes = ctypes.c_void_p, 
  ...: fclose.restype = ctypes.c_int

In [4]: fopen = libc.fopen
  ...: fopen.argtypes = ctypes.c_char_p, ctypes.c_char_p
  ...: fopen.restype = ctypes.c_void_p
````
and similarly it is possible to access the numpy arrays like it is possible with cffi, here the pointer to data could be created as:
````python
In [5]: import numpy as np

In [6]: d = np.ndarray((3,4),dtype=np.uint16)
In [16]: d
Out[16]:
array([[    0,    0,    0, 16352],
      [55050, 28835,  2621, 16343],
      [34079, 20971,  7864, 16341]], dtype=uint16)
In [12]: ptr = ctypes.cast(d.ctypes.data,ctypes.POINTER(ctypes.c_uint16))

In [17]: ptr[3]
Out[17]: 16352
In [18]: ptr[3] = 34 
In [20]: d
Out[20]:
array([[    0,    0,    0,    34],
      [55050, 28835,  2621, 16343],
      [34079, 20971,  7864, 16341]], dtype=uint16)
````
similarly we could build array in python and later build an numpy array:
````python
In [21]: dataptr = (ctypes.c_float*10)()      //
In [29]: data = np.ndarray((5,2),dtype=np.float32,buffer=dataptr)

In [30]: data
Out[30]:
array([[0., 0.],
      [0., 0.],
      [0., 0.],
      [0., 0.],
      [0., 0.]], dtype=float32)

In [34]: dataptr[4] = math.pi

In [35]: data
Out[35]:
array([[0.      , 0.      ],
      [0.      , 0.      ],
      [3.1415927, 0.      ],
      [0.      , 0.      ],
      [0.      , 0.      ]], dtype=float32)
````
To free the loaded library use the windll from _ctypes:
````python
import _ctypes
_ctypes.FreeLibrary(mlib._handle)
````