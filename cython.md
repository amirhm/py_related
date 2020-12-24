# Cython 
Inside a notebook as well we could use the cython to accelerate the python code
To se code implemented on cython first the cython module should be imported inside notebook

`%load_ext cython`

now in the cell that you want to write in the cython just declare at the begining:

## example for reading binary file in regions:
```python
%%cython
import numpy as np
cimport numpy as np
cimport cython
from libc.stdio cimport fread, fopen, fseek, FILE, fclose
 

@cython.boundscheck(False)
@cython.wraparound(False)
def shape_read(str file_adr, (int, int) size, (int,int) corner):
    
    cdef int size_0 = size[0] 
    cdef int size_1 = size[1]
    cdef int corner_0 = corner[0] 
    cdef int corner_1 = corner[1]
    cdef np.uint8_t[:] im = np.zeros(size_0 * size_1 * 3, dtype=np.uint8)
    cdef np.int32_t[:] shape = np.fromfile(file_adr,dtype=np.int32,count=12)
    cdef int im_size_0 = shape[0]
    cdef int im_size_1 = shape[1]
    
    if (size_0 + corner[0] > im_size_0):
        raise ValueError('out of range in Height Dimention')
    if (size_1 + corner[1] > im_size_1):
        raise ValueError('out of range in width Dimention')
    
    cdef int sz = size[0]
    cdef int offset_v = 0
    cdef int j
    
    #cdef np.uint8_t[:] row = np.zeros(size_1 * 3,dtype=np.uint8)
    cdef np.ndarray[np.uint8_t, ndim=1, mode = 'c'] rowbuf = np.ascontiguousarray(im, dtype = np.uint8)
    cdef np.uint8_t* buf_ptr = <np.uint8_t*> rowbuf.data
    
    filename_byte_string = file_adr.encode("UTF-8")
    cdef char* fname = filename_byte_string
    
    cdef FILE *fp
    fp = fopen(fname, "rb")
     
    for j in range(sz):
        offset_v = ((corner_0 + j) * im_size_1 + corner_1)*3
        fseek(fp, 48 + offset_v, 0)
        fread(buf_ptr+(j*(size_1 * 3)), 1, size_1 * 3 , fp)
        #row = np.fromfile(file_adr,
        #                    dtype=np.uint8,
        #                    count=size[1]*3,
        #                    offset= 48 + offset_v)
        #im[j*(size_1 * 3):(j+1)*(size_1 * 3)] = rowbuf
        #im = rowbuf
    fclose(fp)
    return np.reshape(im, (size_0, size_1,3))
``` 

to see the generated c code at begining we can use `-a` flag `%%cython -a`
declare the arrays in the format as for example 

cdef int[:]: 1D array
cdef int[:,:]: 2D array

and so on 

Tuples could be declared as (int,int) for a tuple of Tuple[int,int] and so on

to use the numpy types cimport the numpy and then you can use the type.
To declare the numpy array:
np.ndarray[np.uint8_t, ndim=1, mode = 'c']



