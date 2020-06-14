PIP Local server on ASBUILD01
===============



The local pypiserver is installed on the asbuild01 (on port 8081). To add the repository on the pip.ini file (On Windows, linux pip.conf)
use these lines 

````
[global]
extra-index-url = http://asbuild01:8081/
trusted-host = asbuild01
[install]
trusted-host = asbuild01
````
after these modification you should be able to directly use the `pip insall` to install the local packages.
or if you did not modified the pip.ini then just pass it on the command 
````
pip install --extra-index-url http://asbuild01:8081 --trusted-host asbuild01 ic3019
````

to upload the package after build, you could use the twine package. command will be :

````
twine upload --repository-url http://asbuild01:8081 dist\astools-0.0.*.tar*
````
