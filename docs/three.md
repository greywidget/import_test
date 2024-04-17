# PYTHONPATH
Using the same code as the previous example:

``` powershell
my_project
└── my_package
    ├── colours.py
    ├── main.py
    └── messages.py
```

## The code - as before
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

``` py title="main.py" hl_lines="1 2"
from . import messages
from .colours import rainbow

print(rainbow())
print(messages.message_two())
```

We can manipulate the *environment variable* `PYTHONPATH` so that running `python -m my_package.main` will work regardless of the directory we are in (i.e. we don't have to be in the parent directory of our package)

### Windows Powershell
- `$env:PYTHONPATH` to view the current value
- `$env:PYTHONPATH = "c:\Users\Craig\python_projects\import_test\ex02_packages\my_project\"` to set to my value for this example
- `$env:PYTHONPATH = ""` to unset the *environment variable*

!!! note "Be careful with this"
    Be cautious when setting `PYTHONPATH` because it can potentially override 
    or conflict with system-wide Python installations and other environments.