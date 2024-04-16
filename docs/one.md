# Just Modules

## Modules we would like to import
Using simple modules, you would import like this:
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
    return "A bird in the hand is better than two in the bush (apparently)"
```

## Our main program

``` py title="main.py" hl_lines="1 2"
import colours
import messages

print(colours.dull())
print(messages.message_two())

```

and this runs fine:
``` powershell
python main.py
['beige', 'brown', 'taupe']
A bird in the hand is better than two in the bush (apparently)
```

## Can we import individual functions?
``` py title="main.py" hl_lines="1"
from messages import message_one

print(message_one())
```

yep, that's fine:
``` powershell
python main.py
I came from one

```

## What about importing *?
This does work but it's not recommended. (Ruff complains about it in VsCode)
``` py title="main.py" hl_lines="1"
from messages import *

print(message_one())
print(message_two())
```

yep, that's fine:
``` powershell
python main.py
I came from one
A bird in the hand is better than two in the bush (apparently)

```

## What about relative imports
As `colours` and `messages` are not *packages* we can't do relative imports:
``` py title="main.py" hl_lines="1"
from . import colours

print(colours.dull())
```

``` powerscript
python main.py
Traceback (most recent call last):
  File "C:\Users\Craig\python_projects\import_test\ex01_just_modules\main.py", line 1, in <module>
    from . import colours
ImportError: attempted relative import with no known parent package
```

and similarly, the following is not valid:
``` py title="main.py" hl_lines="1"
from .colours import dull

print(dull())
```

``` powerscript
python main.py
Traceback (most recent call last):
  File "C:\Users\Craig\python_projects\import_test\ex01_just_modules\main.py", line 1, in <module>
    from .colours import dull
ImportError: attempted relative import with no known parent package
```
