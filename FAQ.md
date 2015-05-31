# Frequently asked questions about Multi-Mechanize #


---


### What is the minimum version of Python I should run? ###

2.6 is required<br>
2.6.3+ is reccomended (<a href='http://bugs.python.org/issue6838'>http://bugs.python.org/issue6838</a>)<br>
<br>
<hr />

<h3>Which operating systems are supported?</h3>

It should run anywhere you can install Python and Matplotlib.<br>
Development and testing is mostly done on Ubuntu 64-bit and Windows 7 64-bit.<br>
<br>
I have received reports of people having trouble getting Matplotlib installed on OS X, but don't have an environment to confirm this on.<br>
<br>
<hr />

<h3>Why does it launch multiple OS processes?</h3>

Each "user group" runs in its own process.  Within each process, multiple virtual user agents are run; each in its own thread.  So it uses a hybrid multi-process/multi-threaded architecture.<br>
<br>
<hr />

<h3>How do you record HTTP traffic?</h3>

There is no "recorder" supplied with multi-mechanize.  You have to write the scripts yourself.  Script examples are shipped with the tool to get you started.<br>
<br>
There are also several browser plugins that will help you view HTTP traffic and see what is going on at the network layers:<br>
<ul><li><a href='http://www.fiddler2.com/'>Fiddler</a>
</li><li><a href='http://getfirebug.com/'>Firebug</a>
</li><li><a href='http://code.google.com/p/httpfox/'>HTTPFox</a></li></ul>

<hr />

<h3>How do I install Mechanize on Windows?</h3>

<ul><li>go to <a href='http://pypi.python.org/pypi/distribute#distribute-setup-py'>http://pypi.python.org/pypi/distribute#distribute-setup-py</a>
</li><li>download the distribute-setup-py script<br>
</li><li>run the distribute-setup-py script.  this will install Distribute<br>
</li><li>go to a command prompt at C:\Python26\Scripts<br>
</li><li>run: 'easy_install mechanize'</li></ul>

<hr />

<h2>Where do I get Matplotlib and Numpy for 64 bit windows?</h2>

<blockquote>If you are running Windows 64-bit, I recommend installing Numpy then Matplotlib from here:<br>
</blockquote><ul><li><a href='http://www.lfd.uci.edu/~gohlke/pythonlibs/'>http://www.lfd.uci.edu/~gohlke/pythonlibs/</a></li></ul>

<hr />

<h3>Which packages do I need to install on Ubuntu/Debian?</h3>
If you are running Ubuntu or Debian, you can get all the prerequisites directly from the OS package repository.  Run the following commands:<br>
<ul><li>sudo apt-get install python-mechanize<br>
</li><li>sudo apt-get install python-matplotlib</li></ul>

<hr />

<h3>What are the fields in results.csv?</h3>

results.csv is used for results storage during a test run.  It is a comma separated text file that is used internally to generate results reports.  You might also find the raw file useful for your own analysis.<br>
<br>
the fields correspond to:<br>
<code>transaction count, elapsed time, epoch time, user_group name, script run time, error, custom_timers</code>

<hr />

<h3>Should I keep console_logging on during my tests?</h3>

No, only keep logging on while developing/debugging your scripts.  You should turn it off to get highest throughput.  It takes substantial system resources to print all responses to stdout when doing high amounts of i/o.<br>
<br>
<hr />