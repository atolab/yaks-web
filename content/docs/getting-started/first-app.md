---
title: "Your First Yaks app"
weight : 1020
menu:
  docs:
    parent: getting_started
---
Getting started with Yaks is quite straightforward. Below we will show you how to create a simple telemetry application. Let's assume that we have some sensor, say a temperature sensor, and we want to store this temperature into a Yaks storage. Later on, we want to retrieve this temperature from the Yaks storage. 

Before cranking some code, let's define some terminology. 

<b>Yaks</b> deals with <i>keys/values</i> where each key is a <i>path</i> and is associated to a <i>value</i>. A path looks like just a Unix file system path, such as ```/myhome/kitchen/temp```. The value can be defined with different
encodings (string, JSON, raw bytes buffer...). 

To query the values stored by Yaks, we use <i>selectors</i>. As the name suggest, a <i>selector</i> can uses wildcards, such as <b>*</b> and <b>**</b> to represent a set of paths, such as, ```/myhome/*/temp```.

Let's get started!

## Yaks Programming in Python 

By default, a Yaks service starts without any storage. In order to store the temperature, we need to add one.
The code below create a storage in Yaks memory that will store any key starting with `/myhome/`:

```python
from yaks import Yaks

if __name__ == "__main__":        
    y = Yaks.login(None)
    y.admin().add_storage('mystorage', {'selector': '/myhome/**'})
```


Now let's write an application that will produce temperature measurements at each second:

```python
from yaks import Yaks, Encoding, Value
import random
import time

random.seed()

def read_temp():
    return random.randint(15, 30)    

def run_sensor_loop(w):
    # read and produce e temperature every second
    while True:
        t = read_temp()
        w.put('/myhome/kitcken/temp', Value(str(t), encoding=Encoding.STRING))
        time.sleep(1)

if __name__ == "__main__":        
    y = Yaks.login(None)
    w = y.workspace('/')
    run_sensor_loop(w)
```
 

Below is the application that will retrieve the latest temperature value stored in Yaks:

```python
from yaks import Yaks

if __name__ == "__main__":        
    y = Yaks.login(None)
    w = y.workspace('/')
    results = w.get('/myhome/kitcken/temp')
    key, value = results[0].path, results[0].value
    print('  {} : {}'.format(key, value))
```


Finally, if ever we want to receive the temperatures in direct from the publisher, without querying the Yaks storage,
we can use a subscriber:

```python
from yaks import Yaks, ChangeKind
import time

def listener(changes):
    for change in changes:
        if change.get_kind() == ChangeKind.PUT:
            print('Publication received: "{}" = "{}"'
                  .format(change.get_path(), change.get_value()))

if __name__ == "__main__":        
    y = Yaks.login(None)
    w = y.workspace('/')
    results = w.subscribe('/myhome/kitcken/temp', listener)
    time.sleep(60)
```

## Other code examples

Now you can also have a look to the examples provided with each client API:

 - **Python**: https://github.com/atolab/yaks-python/tree/master/examples
 - **Java**:   https://github.com/atolab/yaks-java/tree/master/examples
 - **Go**:     https://github.com/atolab/yaks-go/tree/master/examples
 