## Database Storage for Test Data and Results ##

Test data and results can be stored to a database when a test run completes. To enable database storage, you must add a **results\_database** option to your config.cfg, which defines the db connection string.

example:
```
results_database: sqlite:///results.db
```

Database storage requires sqlalchemy:
http://www.sqlalchemy.org

Several database back-ends are available to choose from:<br>
<a href='http://www.sqlalchemy.org/docs/dbengine.html#supported-databases'>http://www.sqlalchemy.org/docs/dbengine.html#supported-databases</a>

Example connection strings:<br>
<br>
<table><thead><th>SQLite</th><th><code>sqlite:///dbname</code></th></thead><tbody>
<tr><td>MySQL </td><td><code>mysql://user:password@localhost/dbname</code></td></tr>
<tr><td>PostgreSQL</td><td><code>postgresql://user:password@host:port/dbname</code></td></tr>
<tr><td>MS SQL Server</td><td><code>mssql://mydsn</code>   </td></tr></tbody></table>

<i><code>*</code> SQLite is supported natively by Python, so there is no installation or configuration necessary.</i><br>
<i><code>*</code> The results database is created automatically on first use, no need to run your own DDL code.</i>

<h3>Results Database Diagram</h3>

<img src='http://www.goldb.org/multi-mechanize/results_database_erd.gif'></img>

<h3>Sample DB Query Code</h3>

Here is some sample code for retrieving results from the database via sqlalchemy:<br>
<pre><code>from sqlalchemy import create_engine<br>
from sqlalchemy.orm import sessionmaker<br>
from lib.resultsloader import ResultRow<br>
from lib.resultsloader import GlobalConfig<br>
from lib.resultsloader import UserGroupConfig<br>
from lib.resultsloader import TimerRow<br>
<br>
engine = create_engine('sqlite:///results.db')<br>
session = sessionmaker(bind=engine)<br>
current = session()<br>
<br>
for rr in current.query(ResultRow).order_by(ResultRow.trans_count):<br>
    print rr<br>
    print rr.global_config<br>
    print rr.global_config.user_group_configs<br>
    print rr.timers<br>
</code></pre>