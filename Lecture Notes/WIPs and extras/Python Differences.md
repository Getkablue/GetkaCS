This section will contain notes on how Python diverges from what we have learned.

In Python, variables don't exist until defined, and python does not strictly enforce type. Because Python does not enforce strict typing of variables like C and C++ do, the following is perfectly acceptable:
```python
something = 32 # Integer value
something = "Steve" # String value
something = ['apple', 'banana', 'grape'] # List value
something = [] # empty list value
something = {} # empty dictionary value
something = None # None value
something = 7.001 + 6.96969 # Float values being added, float result
something = int(5.5 + 4.1) # explicitly convert float result to integer
```
(Note that None is *an actual value*, not the absence of a value. A variable with a value of None still exists. It is a common pattern in python to return None if nothing is found.)

But note that *using* a variable before assignment will raise an error in Python:
``` python
print(anotherSomething) # will raise an AttributeError!
```