# Python Notes

## Contents
- [Basic Ideas](#basic-ideas)
- [Useful Packages](#useful-packages)
- [Useful Functions](#useful-functions)
- [Tricks](#tricks)
- [Questions and Exercises](#questions-and-exercises)
- [Pyplot](#pyplot)

   
## Basic Ideas

### Everything is an object in Python !!!

See the following examples:
```python
y = [1,2,3]

def plusOne(y):
    for x in range(len(y)):
        y[x] += 1
    return y

print plusOne(y), y


a = 2

def plusOne2(a):
    a += 1
    return a

print plusOne2(a), a
```
<br/>

Values in `list y` will be changed, while tje value of a won't.

<br/>

Brief explanation can be as this:

<br/>

In Python, all values are __objects__, assignment change the object that the variable __points to__. In other words, there isn't really Variables in Python, instead, they are just __names__, and the `=` operation doesn't assign a value to the variables, it binds (points) a name to an object!

refer:[discussion on stack overflow][sf_name]

[sf_name]:https://stackoverflow.com/questions/17686596/function-changes-list-values-and-not-variable-values-in-python



### The __array__ axes meaning

In `numpy`, the axis (or axes) are defined as 
```
The axis number of the dimension is the index of that dimension within the array's shape. It is also the position used to access that dimension during indexing.
```
For example, `Axis = 0` is the first dimension (the rows) of the array and `Axis = 1` for the second dimension (the columns). However, when it comes to higher dimensions, the "rows" and "columns" don't really mean anything. Instead, think of the axes as the indices of the array's dimensions.

When using `numpy.sum()`, here is the quick result of the 
![array axes explanation](https://github.com/mobydickhm1851/NotesForAll/blob/master/numpy_array.jpg?raw=true)

Sources:[discussion in stackoverflow][stack] and the [image from this post][zihu].

[stack]:https://stackoverflow.com/questions/17079279/how-is-axis-indexed-in-numpys-array
[zihu]:https://zhuanlan.zhihu.com/p/30960190

### Use array or matrix ?

Use array.

Source : [Key Differences between array and matrix][np_arr_mat]

### How to extend or knock out elements in an array ?

Using `__numpy.append()__` function, for example

#### extending

```
>>> import numpy as np
>>> p = np.array([[1,2],[3,4]])

>>> p = np.append(p, [[5,6]], 0)
>>> p = np.append(p, [[7],[8],[9]],1)

>>> p
array([[1, 2, 7],
   [3, 4, 8],
   [5, 6, 9]])
```
 
#### knocking out

```
    p = np.array(range(20))
>>> p.shape = (4,5)
>>> p
array([[ 0,  1,  2,  3,  4],
       [ 5,  6,  7,  8,  9],
       [10, 11, 12, 13, 14],
       [15, 16, 17, 18, 19]])
>>> n = 2
>>> p = np.append(p[:n],p[n+1:],0)
>>> p = np.append(p[...,:n],p[...,n+1:],1)
>>> p
array([[ 0,  1,  3,  4],
       [ 5,  6,  8,  9],
       [15, 16, 18, 19]])
```
Sources: [Stack Overflow answer][array_expend]

[np_arr_mat]:https://docs.scipy.org/doc/numpy-1.14.0/user/numpy-for-matlab-users.html
[array_expend]:https://stackoverflow.com/questions/877479/whats-the-simplest-way-to-extend-a-numpy-array-in-2-dimensions/877564#877564


### Usage at unzipping argument
If you see the following Error message:
```
TypeError: argument after * must be a sequence
```
That means you need to add a ',' after the argument you pass. For example:<br/>
Sending `arg = (s)` is trying to unpack a number of arguments instead of sending just that single arguement. 
The correct way to pass the arg whould be `args=(s,)`.

More information about this, [check this out][unzip]

[unzip]:https://stackoverflow.com/questions/36387596/python-typeerror-argument-after-must-be-a-sequence

## Useful Packages
- Numpy: Python做多維陣列（矩陣）運算時的必備套件，比起Python內建的list，Numpy的array有極快的運算速度優勢
- Pandas：有了Pandas可以讓Python很容易做到幾乎所有Excel的功能了，像是樞紐分析表、小記、欄位加總、篩選
- Matplotlib：基本的視覺化工具，可以畫長條圖、…
- Seaborn：另一個知名的視覺化工具，個人認為畫起來比matplotlib好看
- SciKit-Learn: Python 關於機器學習的model基本上都在這個套件，像是SVM, Random Forest…

Source:[Teh James Medium][1]

[1]:https://medium.com/@yehjames/%E8%B3%87%E6%96%99%E5%88%86%E6%9E%90-%E6%A9%9F%E5%99%A8%E5%AD%B8%E7%BF%92-%E7%AC%AC%E4%B8%80%E8%AC%9B-python%E6%87%B6%E4%BA%BA%E5%8C%85-anaconda-%E4%BB%8B%E7%B4%B9-%E5%AE%89%E8%A3%9D-f8199fd4be8c



## Useful Functions
- Random numbers with normal distribution

For random samples from `N(mu, sigma^2)`, use:
```python
sigma * np.random.randn(...) + mu
```

__Sources__:[numpy.org][numpy], also see [Scipy.org][scipy] for all the `random` function.

- Get keyboard input
The following code get keyboard input and return a string :
```python
def getKey():
    fd = sys.stdin.fileno()
    old_settings = termios.tcgetattr(fd)
    try:
        tty.setraw(sys.stdin.fileno())
        key = sys.stdin.read(1)
    finally:
        termios.tcsetattr(fd, termios.TCSADRAIN, old_settings)
    return key
```

The system will be waiting for an input, so if we want the program keep running whether there is an input or not, modify it into this :

```python
def getKey():

    tty.setraw(sys.stdin.fileno())
    rlist, _, _ = select.select([sys.stdin], [], [], 0.1)
    # The fellowing if keep system from waiting for input
    if rlist:
        key = sys.stdin.read(1)
    else:
        key = ''

    termios.tcsetattr(sys.stdin, termios.TCSADRAIN, settings)
    return key
```
References:[0][0][1][1][2][2]

[0]:https://stackoverflow.com/questions/510357/python-read-a-single-character-from-the-user
[1]:https://www.jianshu.com/p/b6b59ec1fa12
[2]:https://hk.saowen.com/a/6daf2b56d2580ed55fbdc56afe4355c64e20bce1fde7d60259304220e751535b




[numpy]:https://www.numpy.org/devdocs/reference/generated/numpy.random.randn.html
[scipy]:https://docs.scipy.org/doc/numpy-1.13.0/reference/routines.random.html


## Tricks
### Simplification for loops
- `List Comprehension`
``` python
def list_doubler(lst):
    doubled = []
    for num in lst:
        doubled.append(num*2)
    return doubled
    
# using list comprehension
def list_doubler(lst):
    return [num * 2 for num in lst]
```
Used with __conditions__
```python
def long_words(lst):
    words = []
    for word in lst:
        if len(word) > 5:
           words.append(word)
    return words

#using list comprehension
def long_words(lst):
    return [word for word in lst if len(word) > 5]
```

- `Dictionary Comprehension`
- `Set Comprehension`

Source:[Python single line for loop][lc]

[lc]:https://blog.teamtreehouse.com/python-single-line-loops



## Questions and Exercises
- [50+ Data Structure and Algorithms Interview Questions for Programmers][50+]

[50+]:https://hackernoon.com/50-data-structure-and-algorithms-interview-questions-for-programmers-b4b1ac61f5b0


## Pyplot
### Adding chinese font

1. First you'll have to find the `.ttf` file of the font. (If you can only find `.ttc` file, which is a collection of fonts, use [Transfonter](https://transfonter.org/ttc-unpack) to unpack `.ttc` files.)

2. Locate the folder to add `.ttf` into :
   ```python
   import matplotlib
   print(matplotlib.matplotlib_fname())
   ```
   which should be `~/.local/lib/pythonx.x/site-packages/matplotlib/mpl-data/matplotlibrc`, and the font folder will be at `~/.local/lib/pythonx.x/site-packages/matplotlib/mpl-data/fonts/ttf`

3. Add the `.ttf` into it and rebuild the catche
   ```python
   import matplotlib.font_manager
   matplotlib.font_manager._rebuild()
   ```

4. Find the font name we're going to add into the python script:
   ```python
   >> [f for f in matplotlib.font_manager.fontManager.ttflist if 'XYZ' in f.name]
   ```
   where `XYZ` should be the keyword of the font to be added.

5. Let's say the font name found in `fontManager` is `XYZ TW`, then in the python script:
   ```
   from matplotlib import rcParams
   rcParams['font.family'] = ['XYZ TW']
   ```

Source:[Kelvin Ng](https://medium.com/@hoishing), [StackOverFlow](https://stackoverflow.com/questions/21321670/how-to-change-fonts-in-matplotlib-python)

### Plot or savefig error message
Under Ubuntu 18.04 and python 3.6, the error message is encountered when using `matplotlib.pyplot.show()` or `matplotlib.pyplot.savefig()`:

```
matplotlib/backends/backend_gtk3.py:215: Warning: Source ID 7 was not found when attempting to remove it GLib.source_remove(self._idle_event_id)
```

#### Fix for me (but might encounter error in the future...)
Go to `~/.local/lib/python3.6/site-packages/matplotlib/backends/backend_gtk3.py` and edit as following:
```diff
def destroy(self):
    #Gtk.DrawingArea.destroy(self)
    self.close_event()
-   if self._idle_draw_id != 0:
-      GLib.source_remove(self._idle_draw_id)
```

For more information, check this [thread in StackOverFlow](https://stackoverflow.com/questions/29540845/how-can-i-deactivate-warning-source-id-510-was-not-found-when-attempting-to-re)
