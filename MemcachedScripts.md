## Multi-Mechanize Virtual User Scripts for Memcached/Membase ##

Below is an example Multi-Mechanize script using [python-memcached](http://pypi.python.org/pypi/python-memcached) to stress test a memcache or membase server/cluster:

```
import random
import time
import memcache



NODES = ['192.168.10.2:11211',]
DATA_SIZE = 30000
WAIT_TIME = .1
KEY_RANGE = (1, 1000000)



class Transaction(object):
    def __init__(self):
        self.mc = memcache.Client(NODES)
        self.custom_timers = {}
    
    def run(self):
        key = str(random.randint(*KEY_RANGE))
        data = '*' * DATA_SIZE
        
        start_timer = time.time()
        self.mc.set(key, data)
        stop_timer = time.time()
        
        self.custom_timers['SET'] = stop_timer - start_timer
        
        start_timer = time.time()
        self.mc.get(key)
        stop_timer = time.time()
        
        self.custom_timers['GET'] = stop_timer - start_timer
        
        time.sleep(WAIT_TIME)
       
 

if __name__ == '__main__':
    trans = Transaction()
    trans.run()
    print trans.custom_timers
```