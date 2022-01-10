---
layout: post
title:  "MySQL Performance Schema"
categories: mysql
---
Excerpt from MySQL doumentation + some additional data:

The Performance Schema provides a way to inspect internal execution of the server at runtime. It is implemented using the PERFORMANCE_SCHEMA storage engine and the performance_schema database. The Performance Schema focuses primarily on performance data. This differs from INFORMATION_SCHEMA, which serves for inspection of metadata. The MySQL sys schema is a set of objects that provides convenient access to data collected by the Performance Schema. The sys schema is installed by default. 

{% highlight sql %}
show databases;
{% endhighlight %}
|information_schema|
|performance_schema|
|sys|

There are tons of tables:
{% highlight sql %}
SELECT table_name FROM information_schema.tables
    WHERE table_schema = 'performance_schema';
SHOW TABLES FROM performance_schema;	
{% endhighlight %}
 
| table_name|
|-----------|
| accounts  |
| cond_instances   |
| events_stages_current |
| events_stages_history |
| ... |

Tables in the performance_schema database can be grouped according to the type of information in them: Current events, event histories and summaries, object instances, and setup (configuration) information. Initially, not all instruments and consumers are enabled, so the performance schema does not collect all events.

In seconds.
SHOW STATUS LIKE '%uptime%';
|Uptime	| 1347416 |
| Uptime_since_flush_status	| 1347416 |

The performance_schema.events_statements_summary_by_digest table is a sized table in memory within the Performance Schema, and its size is auto-configured. To check the current size:

mysql> SHOW GLOBAL VARIABLES LIKE 'performance_schema_digests_size';
+---------------------------------+-------+
| Variable_name                   | Value |
+---------------------------------+-------+
| performance_schema_digests_size | 5000  |
+---------------------------------+-------+

If your application executes more than this number of normalized statements, then it is possible that you may begin losing some statement instrumentation. You can monitor this situation with the Performance_schema_digest_lost variable:

mysql> SHOW GLOBAL STATUS LIKE 'Performance_schema_digest_lost';
+--------------------------------+-------+
| Variable_name                  | Value |
+--------------------------------+-------+
| Performance_schema_digest_lost | 0     |
+--------------------------------+-------+