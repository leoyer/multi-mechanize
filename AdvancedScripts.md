## Advanced Multi-Mechanize Virtual User Scripts ##

#### wikipedia search with form fill/submit, timers, assertions, custom headers, think-times ####
```
import mechanize
import time


class Transaction(object):
    def __init__(self):
        self.custom_timers = {}
    
    def run(self):
        # create a Browser instance
        br = mechanize.Browser()
        # don't bother with robots.txt
        br.set_handle_robots(False)
        # add a custom header so wikipedia allows our requests
        br.addheaders = [('User-agent', 'Mozilla/5.0 Compatible')]
        
        # start the timer
        start_timer = time.time()
        # submit the request
        resp = br.open('http://www.wikipedia.org/')
        resp.read()
        # stop the timer
        latency = time.time() - start_timer
        
        # store the custom timer
        self.custom_timers['Load_Front_Page'] = latency  
        
        # verify responses are valid
        assert (resp.code == 200), 'Bad HTTP Response'
        assert ('Wikipedia, the free encyclopedia' in resp.get_data()), 'Text Assertion Failed'
        
        # think-time
        time.sleep(2)  
        
        # select first (zero-based) form on page
        br.select_form(nr=0)
        # set form field        
        br.form['search'] = 'foo'  
        
        # start the timer
        start_timer = time.time()
        # submit the form
        resp = br.submit()  
        resp.read()
        # stop the timer
        latency = time.time() - start_timer
        
        # store the custom timer
        self.custom_timers['Search'] = latency  
        
        # verify responses are valid
        assert (resp.code == 200), 'Bad HTTP Response'
        assert ('foobar' in resp.get_data()), 'Text Assertion Failed'
        
        # think-time
        time.sleep(2)  
```


#### using urllib2 directly instead of mechanize ####
this example will generate an HTTP GET
```
import urllib2
import time


class Transaction(object):
    def __init__(self):
        self.custom_timers = {}
    
    def run(self):
        start_timer = time.time()
        resp = urllib2.urlopen('http://www.example.com/')
        content = resp.read()
        latency = time.time() - start_timer
        
        self.custom_timers['Example_Homepage'] = latency
        
        assert (resp.code == 200), 'Bad HTTP Response'
        assert ('Example Web Page' in content), 'Failed Content Verification'
```

#### using urllib2 directly instead of mechanize ####
this example will generate an HTTP POST containing a SOAP request in its body.  The request message (SOAP envelope) is read from a file named 'soap.txt'.
```
import urllib2
import time


class Transaction(object):
    def __init__(self):
        self.custom_timers = {}
        with open('soap.xml') as f:
            self.soap_body = f.read()
    
    def run(self):
        req = urllib2.Request(url='http://www.foo.com/service', data=self.soap_body)
        req.add_header('Content-Type', 'application/soap+xml')
        req.add_header('SOAPAction', 'http://www.foo.com/action')

        start_timer = time.time()
        resp = urllib2.urlopen(req)
        content = resp.read()
        latency = time.time() - start_timer
        
        self.custom_timers['Example_SOAP_Msg'] = latency
        
        assert (resp.code == 200), 'Bad HTTP Response'
        assert ('Example SOAP Response' in content), 'Failed Content Verification'
```

#### using httplib directly instead of mechanize ####
```
import httplib
import urllib
import time


class Transaction(object):
    def __init__(self):
        self.custom_timers = {}
    
    def run(self):
        post_body=urllib.urlencode({
            'USERNAME': 'corey',
            'PASSWORD': 'secret',})
        headers = {'Content-type': 'application/x-www-form-urlencoded'}            
        
        start_timer = time.time()
        conn = httplib.HTTPConnection('www.example.com')
        conn.request('POST', '/login.cgi', post_body, headers)
        resp = conn.getresponse()
        content = resp.read()
        latency = time.time() - start_timer
        
        self.custom_timers['LOGIN'] = latency
        
        assert (resp.status == 200), 'Bad HTTP Response'
        assert ('Example Web Page' in content), 'Failed Content Verification'
```

#### using httplib, with detailed timing ####
```
import httplib
import time


class Transaction(object):
    def __init__(self):
        self.custom_timers = {}
    
    def run(self):
        conn = httplib.HTTPConnection('www.example.com')
        start = time.time()
        conn.request('GET', '/')
        request_time = time.time()
        resp = conn.getresponse()
        response_time = time.time()
        conn.close()     
        transfer_time = time.time()
        
        self.custom_timers['request sent'] = request_time - start
        self.custom_timers['response received'] = response_time - start
        self.custom_timers['content transferred'] = transfer_time - start
        
        assert (resp.status == 200), 'Bad HTTP Response'

        

if __name__ == '__main__':
    trans = Transaction()
    trans.run()
    
    for timer in ('request sent', 'response received', 'content transferred'):
        print '%s: %.5f secs' % (timer, trans.custom_timers[timer])
```