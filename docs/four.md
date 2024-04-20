# Namespace Packages
A namespace package is a composite of various *portions* (a set of files in a
single directory), where each portion contributes a subpackage to the parent
package. See [The import system - Namespace Packages](https://docs.python.org/3/reference/import.html#namespace-packages)

Thanks to [Ajyssa Coghlan's Note re import traps](https://python-notes.curiousefficiency.org/en/latest/python_concepts/import_traps.html)
for the following example:

Consider the following structure:
```bash
ex03_namespace_packages/
├── project1
│   └── example
│       └── foo.py
└── project2
    └── example
        └── bar.py
```
`foo.py` and `bar.py` *both* look like this:
``` python
print("Hello from ", __name__)
```
Notice that:
- There are no `__init__.py` files
- `project1` and `project2` both have a subdirectory called `example`

`example` here is a `Namespace Package`.

## Our `example` package has portions from multiple locations
Let's add `project1` and `project2` to `PYTHONPATH`:
``` powershell
$env:PYTHONPATH = "C:\Users\Craig\python_projects\import_test\ex03_namespace_packages\project1;C:\Users\Craig\python_projects\import_test\ex03_namespace_packages\project2"
```

If we go into the REPL, we can see that these two directories are in `sys.path`:
``` python
>>> import sys
>>> for item in sys.path:
...     print(item)
C:\Users\Craig\python_projects\import_test\ex03_namespace_packages\project1
C:\Users\Craig\python_projects\import_test\ex03_namespace_packages\project2
C:\Users\Craig\AppData\Local\Programs\Python\Python312\python312.zip
C:\Users\Craig\AppData\Local\Programs\Python\Python312\DLLs
C:\Users\Craig\AppData\Local\Programs\Python\Python312\Lib
C:\Users\Craig\AppData\Local\Programs\Python\Python312
C:\Users\Craig\python_projects\import_test\.venv
C:\Users\Craig\python_projects\import_test\.venv\Lib\site-packages
>>>
```

Let's try importing our `example` *Namespace Package* - if we look at its 
`__path__` attribute, we will see that it includes *both* our projects:
``` python
import example
>>> for item in example.__path__:
...     print(item)
C:\Users\Craig\python_projects\import_test\ex03_namespace_packages\project1\example
C:\Users\Craig\python_projects\import_test\ex03_namespace_packages\project2\example
>>>
```

## We can import modules from any of the locations
So, our `example` (Namespace) package has portions coming from multiple
locations. We can prove that it works by importing our two python modules:
``` python
>>> import example.foo
Hello from example.foo
>>> from example import bar
Hello from example.bar
```

## Here's the Rub
If we add a `__init__.py` file to `example.py` in either of our projects, the
Python interpreter will create a *single directory* package containing only
modules from that directory, rather than finding all appropriately named
subdirectories. Lets add one. Our structure now looks like this:
``` bash
ex03_namespace_packages/
├── project1
│   └── example
│       └── foo.py
└── project2
    └── example
        ├── __init__.py
        └── bar.py
```
We have not changed anything else, to both projects are in `sys.path`. If we
import now:
``` python
>>> from example import foo
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
ImportError: cannot import name 'foo' from 'example' (C:\Users\Craig\python_projects\import_test\ex03_namespace_packages\project2\example\__init__.py)
>>> from example import bar
Hello from example.bar
>>>
```
We can no longer access `foo.py` because adding `__init__.py` to `project2/example`
has turned `example` into a *single directory* package e.g. a *normal* package.

Most of the time, we only create *normal* packages and so it is a good practice
to *always create `__init__.py`*
