# Remote Management API #


---


You can invoke multi-mechanize with an optional `port` command-line
option, like this:
```
>python multi-mechanize.py default_project --port 9000 
```
or
```
>python multi-mechanize.py default_project -p 9000
```

When you specify a port, rather than starting a test, it will launch
a server and listen for remote commands.  You can then control it over
HTTP.

The XML-RPC API supports the following methods:
```
  run_test() 
  check_test_running() 
  update_config(config) 
  get_config() 
  get_project_name() 
  get_results() 
```

sample remote client code for interacting
with multi-mechanize:

```
#!/usr/bin/env python
 
import xmlrpclib
 
server = xmlrpclib.ServerProxy('http://foo.example.com:9000') 

print server.get_project_name()
print server.get_config() 
server.run_test() 
```