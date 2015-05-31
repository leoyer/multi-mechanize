## Virtual User Script Development Guide ##

under construction.


---


Scripts are written in Python.  You have full access to the Python standard library and any other additional modules you have installed.


---


For more advanced scripting examples, see: AdvancedScripts


---


Each script must implement a `Transaction()` class.  The class must implement a `run()` method.

So a basic test script consists of:

```
class Transaction:
    def run(self):
        # do something here
```

During a test run, your `Transaction()` class is instantiated once, and then its `run()` method is called repeatedly in a loop:

```
class Transaction:
    def __init__(self):
        # this gets called once
    
    def run(self):
        # this gets called repeatedly
```

A full script that will issue an HTTP GET using mechanize:

```
import mechanize

class Transaction:    
    def run(self):
        br = mechanize.Browser()
        br.set_handle_robots(False)    
        resp = br.open('http://www.example.com/')
        resp.read()
```

This script adds response verifications:

```
import mechanize

class Transaction:
    def run(self):
        br = mechanize.Browser()
        br.set_handle_robots(False)
        
        resp = br.open('http://www.example.com/')
        resp.read()
       
        assert (resp.code == 200), 'Bad HTTP Response'
        assert ('Example Web Page' in resp.get_data()), 'Failed Content Verification'
```

This script uses a custom timer.  Custom timers are used to wrap blocks of code for timing purposes:

```
import mechanize
import time

class Transaction:
    def __init__(self):
        self.custom_timers = {}
    
    def run(self):
        br = mechanize.Browser()
        br.set_handle_robots(False)
        
        start_timer = time.time()
        resp = br.open('http://www.example.com/')
        resp.read()
        latency = time.time() - start_timer
        
        self.custom_timers['Example_Homepage'] = latency
```