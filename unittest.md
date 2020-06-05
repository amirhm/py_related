writing unit tests in python is done by unittest module (by default is installed)


the file of the test for a module is usually started with test_[name of module to be tested]. and the class could be written as follow:

```python
class TestSomeFunc(unittest.TestCase):
    def test_case1(self):
        self.self.assertAlmostEqual(func(inuts),theexpectedvalue)
        self.assertRaises(ValueError,func, badvalue)

```
to run 

```
python -m unittest nameoffile.py
```
