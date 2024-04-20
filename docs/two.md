# Packages
Packaging seems to be a mixture of the straightforward and the bewildering:

- Namespacing seems to make sense
- Do you or don't you need `__init__.py`?
- Is an installed package different to just being in my package project directory?

Lets look at the following simple project structure:

``` powershell
my_project
└── my_package
    ├── __init__.py
    ├── colours.py
    ├── main.py
    └── messages.py
```

*Notice there IS a `__init__.py` file in the `my_package` directory*  

Although it might not be *necessary* to have a `__init__.py` file, it is best to 
understand the implications. This is discussed in [Namespace Packages](four.md)

## The utility modules are the same as before
``` py title="colours.py"
def rainbow():
    return "red orange yellow green blue indigo violet".split()

def dull():
    return "beige brown taupe".split()
```

``` py title="messages.py"
def message_one():
    return "I came from one"

def message_two():
    return "A bird in the hand is worth two in the bush (apparently)"
```

## But our main program is doing some relative imports

``` py title="main.py" hl_lines="1 2"
from . import messages
from .colours import rainbow

print(rainbow())
print(messages.message_two())
```

## The relative import error again
If we navigate to `my_package` where the modules are, and try running `main.py` we get the error we saw earlier:

```
python main.py
Traceback (most recent call last):
  File "C:\Users\Craig\python_projects\import_test\ex02_packages\my_project\my_package\main.py", line 1, in <module>
    from . import messages
ImportError: attempted relative import with no known parent package
```

The same thing happens if we try to import `main` when we are in that directory:
``` python
>>> import main
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "C:\Users\Craig\python_projects\import_test\ex02_packages\my_project\my_package\main.py", line 1, in <module>
    from . import messages
ImportError: attempted relative import with no known parent package
```

## Let's try again from the *parent* directory:
If we navigate to the parent directory `my_project` and try the import again:
``` python
>>> import main
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
ModuleNotFoundError: No module named 'main'
```

### Our module is now in a child directory
Of course that didn't work - `main.py` is inside our `my_package` directory. Let's try again:
``` python
>>> from my_package import main
['red', 'orange', 'yellow', 'green', 'blue', 'indigo', 'violet']
A bird in the hand is worth two in the bush (apparently)
```
This time our import works. We could also import like this:
``` python
>>> import my_package.main
['red', 'orange', 'yellow', 'green', 'blue', 'indigo', 'violet']
A bird in the hand is worth two in the bush (apparently)
```

## So, what's the deal with the parent directory thing?
Running your code from the *top-level* directory works because it provides Python with a clear reference point for package structure. When you execute a script from the top-level directory of your project, Python considers that directory as the starting point for resolving package imports.

## But what about running it rather than importing it
```
python my_package\main.py
Traceback (most recent call last):
  File "C:\Users\Craig\python_projects\import_test\ex02_packages\my_project\my_package\main.py", line 1, in <module>
    from . import messages
ImportError: attempted relative import with no known parent package
```
No joy there, but actually we can run it like this:

## Using `python -m`
```
python -m my_package.main
['red', 'orange', 'yellow', 'green', 'blue', 'indigo', 'violet']
A bird in the hand is worth two in the bush (apparently)
```
### Module execution vs Script execution
- When you run `python package1\module1.py`, you're directly executing the script file module1.py as if it were a standalone script. Python treats it as a script execution, and the script file is executed in its own namespace.
- When you run `python -m package1.module1`, you're telling Python to run the module module1 as if it were a part of a package. Python treats it as a module execution, and the module is executed in the context of its package.
### Python path handling
- When you run `python package1\module1.py`, Python adds the directory containing `module1.py` to the beginning of `sys.path`. This means that the directory containing `module1.py` becomes the first place where Python looks for modules when resolving imports.
- When you run `python -m package1.module1`, Python doesn't modify `sys.path`. Instead, it looks for the module module1 within the package package1, starting from the current directory.
### Relative imports behaviour
- When you run `python package1\module1.py`, relative imports within `module1.py` are resolved based on the directory containing `module1.py`.
- When you run `python -m package1.module1`, relative imports within `module1.py` are resolved based on the package structure. The top-level package (package1 in this case) is considered the starting point for relative imports.