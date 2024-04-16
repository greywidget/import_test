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
