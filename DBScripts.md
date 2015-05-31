## Multi-Mechanize Virtual User Scripts for Databases ##

Since you can write plugins in Python, you have access to modules for database connections.  You can write scripts that use any database adapter available to stress test a database.

Below is an example using [pymssql](http://pymssql.sourceforge.net/) to stress test a MS SQL Server database:

```
import _mssql
import time



class Transaction(object):
    def __init__(self):
        self.conn = _mssql.connect(server='192.168.1.2', user='sa', password='secret', database='foo', max_conn=999999)                     
                                  
    def run(self):
        start_timer = time.clock()
        self.conn.execute_non_query('exec spMyProc')  
        self.custom_timers['Example_Timer'] = (time.clock() - start_timer)


 
if __name__ == '__main__':
    trans = Transaction()
    trans.run()
    print trans.custom_timers
```