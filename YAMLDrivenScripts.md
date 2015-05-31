## Multi-Mechanize Virtual User Scripting - Data Driven Test Using YAML ##

Example YAML file (wikipedia\_test.yml):

```
WikipediaTest:
  - first_page:
      url: http://www.wikipedia.org
      name: Load_Front_Page
      assert: Wikipedia, the free encyclopedia

  - second_page:
      url: http://en.wikipedia.org
      name: Load_English_Home
      assert: lang="en"

  - third_page:
      url: http://en.wikipedia.org/wiki/Portal:Contents
      name: Load_Portal
      assert: Overviews of Wikipedia
```

Example Multi-Mechanize script using [YAML](http://pyyaml.org) for data-driven testing:

```
import time
import mechanize
import yaml



class Transaction(object):
    def __init__(self):
        self.custom_timers = {}
        
        with open('wikipedia_test.yml') as f:
            self.test_data = yaml.load(f)
        
        
    def run(self):
        # create a Browser instance
        br = mechanize.Browser()
        # don't bother with robots.txt
        br.set_handle_robots(False)
        # add a custom header so wikipedia allows our requests
        br.addheaders = [('User-agent', 'Mozilla/5.0 Compatible')]

        # run the tests (load pages)
        self.test_name = self.test_data.keys()[0]
        for test in self.test_data[self.test_name]:
            for page_name, params in test.iteritems():
                self.load_test_page(br, params['url'], params['name'], params['assert'])
                # user wait time
                time.sleep(2)


    def load_test_page(self, browser, url, page_name, assertion_string):
        #start_timer = time.time() # *nix
        start_timer = time.clock() # Windows
        
        # submit the request
        resp = browser.open(url)
        resp.read()

        # stop the timer
        #latency = time.time() - start_timer  # *nix
        latency = time.clock() - start_timer  # Windows

        # store the custom timer
        self.custom_timers[page_name] = latency

        # verify responses are valid
        assert (resp.code == 200), 'Bad HTTP Response'
        assert (assertion_string in resp.get_data()), 'Text Assertion Failed'
        
        
        
if __name__ == '__main__':
    trans = Transaction()
    trans.run()
    print trans.custom_timers
```

> - requires python-yaml package<br>
<blockquote>- script contributed by Jonathan Kohl (YAML example) - Dec 2010