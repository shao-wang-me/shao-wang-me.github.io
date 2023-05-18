---
publish: true
en_title: Python 3 bool is actually int
---

`bool` 属于 `int` 的子类。

`bool` is a subclass of `int`.

```python
>>> from sys import getsizeof as f
>>> f(1)
28
>>> f(True)
28
>>> f(0)
24
>>> f(False)
24
>>> 0 == False
True
>>> 1 == True
True
>>> True + 1
2
>>> bin(True)
'0b1'
>>> bin(False)
'0b0'
>>> isinstance(False, bool)
True
>>> isinstance(False, int)
True
```

参考：[python - Is False == 0 and True == 1 an implementation detail or is it guaranteed by the language? - Stack Overflow](https://stackoverflow.com/questions/2764017/is-false-0-and-true-1-an-implementation-detail-or-is-it-guaranteed-by-the)