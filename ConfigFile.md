## Configuration File Format ##

Each project contains a config.cfg file where you define settings for a test.

Here is a sample config file showing all possible configuration options:

```
[global]
run_time: 300
rampup: 300
results_ts_interval: 30

progress_bar: on
console_logging: off
xml_report: off
results_database: sqlite:///projects/default_project/results.db
post_run_script: python projects/default_project/foo.py


[user_group-1]
threads: 30
script: example_mock.py

[user_group-2]
threads: 30
script: example_mock.py
```


### Global Options ###

  * **run\_time**: duration of test (seconds)
  * **rampup**: duration of user rampup (seconds)
  * **results\_ts\_interval**: time series interval for results analysis (seconds)
  * **progress\_bar**: turn on/off console progress bar during test run
  * **console\_logging**: turn on/off logging to stdout
  * **xml\_report**: turn on/off xml/jtl report
  * **results\_database**: database connection string **(optional)**
  * **post\_run\_script**: hook to call a script at test completion **(optional)**

### User Groups ###
  * **threads**: number of threads/virtual users
  * **script**: virtual user test script to run