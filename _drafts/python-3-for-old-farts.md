---
title: "Python 3 for Old Farts"
date: 2022-05-30 16:10:08 -0700
layout: post
tags: python
---

It's been many years since I've done any significant coding in Python. Although I do know better than to use Python 2 if I don't have to, I haven't really been paying attention to a lot of the new features that have been added to the language. I recently read through the [What's New in Python](https://docs.python.org/3/whatsnew/index.html) articles and there's a lot of cool stuff. I'll summarize some of the more interesting-to-me user-facing changes for the benefit of other old farts who haven't been paying attention either. (I'm going to skip over the [Python 2 -> 3 changes](https://docs.python.org/3/whatsnew/3.0.html), which have been written about ad nauseum.)

Note that all the examples and some of the descriptions are straight from the What's New docs or official documentation.

### Ordered Dictionaries

Use [collections.OrderedDict](https://docs.python.org/3/library/collections.html#collections.OrderedDict) to iterate over key/value pairs in the order in which they were inserted. (New in version 3.1)

### Changes to `with`

`with` allows multiple context managers in a single statement (version 3.1):

```python
>>> with open('mylog.txt') as infile, open('a.out', 'w') as outfile:
...     for line in infile:
...         if '<critical>' in line:
...             outfile.write(line)
```

### Executable ZIP Files

The ability to create executable ZIP files of Python code has actually been around since version 2.6, but (a) this was news to me, and (b) it's been made a lot more user-friendly since version 3.5. See [PEP 441](https://peps.python.org/pep-0441/) for more info.

```bash
% mkdir test
% echo 'print("Hello world!")' > test/__main__.py
% python3 -m zipapp test -p "/usr/bin/env python3"
% file test.pyz
test.pyz: Zip archive data, made by v2.0 UNIX, extract using at least v2.0, last modified Tue Jan 20 17:01:08 2015, uncompressed size 22, method=store
% chmod a+x test.pyz
% ./test.pyz
Hello world!
```

### concurrent.futures

(New 3.2) [concurrent.futures](https://docs.python.org/3/library/concurrent.futures.html) is a module containing classes for doing asynchronous execution, either with threads or processes. An example:

```python
import concurrent.futures, shutil
with concurrent.futures.ThreadPoolExecutor(max_workers=4) as e:
    e.submit(shutil.copy, 'src1.txt', 'dest1.txt')
    e.submit(shutil.copy, 'src2.txt', 'dest2.txt')
    e.submit(shutil.copy, 'src3.txt', 'dest3.txt')
    e.submit(shutil.copy, 'src3.txt', 'dest4.txt')
```

### functools.lrucache()

(New in 3.2) [functools.lru_cache()](https://docs.python.org/3/library/functools.html#functools.lru_cache) is a decorator for adding caching to function calls.

```python
import functools
>>> @functools.lru_cache(maxsize=300)
... def get_phone_number(name):
...     c = conn.cursor()
...     c.execute('SELECT phonenumber FROM phonelist WHERE name=?', (name,))
...     return c.fetchone()[0]
```
```python
for name in user_requests:
...     get_phone_number(name)        # cached lookup
```

### asyncio

(New in 3.4) [asyncio](https://docs.python.org/3/library/asyncio.html#module-asyncio) is a library to write concurrent code using the async/await syntax:

```python
import asyncio

async def main():
    print('Hello ...')
    await asyncio.sleep(1)
    print('... World!')

asyncio.run(main())
```

### enum

(New in 3.4) The [enum](https://docs.python.org/3/library/enum.html#module-enum) module provides, well, enums:

```python
from enum import Enum

>>> # class syntax
>>> class Color(Enum):
...     RED = 1
...     GREEN = 2
...     BLUE = 3

>>> # functional syntax
>>> Color = Enum('Color', ['RED', 'GREEN', 'BLUE'])
```

### statistics

(New in 3.4) Who needs NumPy? The [statistics](https://docs.python.org/3/library/statistics.html#module-statistics) module provides basic statistics functions.

### typing

(New in 3.5) The [typing](https://docs.python.org/3/library/typing.html#module-typing) module provides type hints:

```python
def greeting(name: str) -> str:
    return 'Hello ' + name
```

and type aliases:

```python
Vector = list[float]

def scale(scalar: float, vector: Vector) -> Vector:
    return [scalar * num for num in vector]

# passes type checking; a list of floats qualifies as a Vector.
new_vector = scale(2.0, [1.0, -4.2, 5.4])
```

and a lot of other fun things, including generics. [PEP 483 - The Theory of Type Hints](https://peps.python.org/pep-0483/) is a good introduction.

### Formatted string literals

(New in 3.6) Oh yeah, time to drop some f-bombs...er, f-strings:

```python
name = "Fred"
>>> f"He said his name is {name}."
'He said his name is Fred.'
>>> width = 10
>>> precision = 4
>>> value = decimal.Decimal("12.34567")
>>> f"result: {value:{width}.{precision}}"  # nested fields
'result:      12.35'
```

### secrets

(New in 3.6) Use the [secrets](https://docs.python.org/3/library/secrets.html#module-secrets) module (not [random](https://docs.python.org/3/library/random.html#module-random)!) to generate cryptographically strong random numbers for anything security-related.

