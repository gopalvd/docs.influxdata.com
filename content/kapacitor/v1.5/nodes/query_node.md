---
title: QueryNode
note: Auto generated by tickdoc

menu:
  kapacitor_1_5:
    name: QueryNode
    identifier: query_node
    weight: 220
    parent: nodes
---
### Constructor

| Chaining Method | Description |
|:---------|:---------|
| **[query](#description)&nbsp;(&nbsp;`q`&nbsp;`string`)** | The query to execute. Must not contain a time condition in the `WHERE` clause or contain a `GROUP BY` clause. The time conditions are added dynamically according to the period, offset and schedule. The `GROUP BY` clause is added dynamically according to the dimensions passed to the `groupBy` method.  |

### Property Methods

| Setters | Description |
|:---|:---|
| **[align](#align)&nbsp;(&nbsp;)** | Align start and stop times for quiries with even boundaries of the QueryNode.Every property. Does not apply if using the QueryNode.Cron property.  |
| **[alignGroup](#aligngroup)&nbsp;(&nbsp;)** | Align the group by time intervals with the start time of the query  |
| **[cluster](#cluster)&nbsp;(&nbsp;`value`&nbsp;`string`)** | The name of a configured InfluxDB cluster. If empty the default cluster will be used.  |
| **[cron](#cron)&nbsp;(&nbsp;`value`&nbsp;`string`)** | Define a schedule using a cron syntax.  |
| **[every](#every)&nbsp;(&nbsp;`value`&nbsp;`time.Duration`)** | How often to query InfluxDB.  |
| **[fill](#fill)&nbsp;(&nbsp;`value`&nbsp;`interface{}`)** | Fill the data. Options are:  |
| **[groupBy](#groupby)&nbsp;(&nbsp;`d`&nbsp;`...interface{}`)** | Group the data by a set of dimensions. Can specify one time dimension.  |
| **[groupByMeasurement](#groupbymeasurement)&nbsp;(&nbsp;)** | If set will include the measurement name in the group ID. Along with any other group by dimensions.  |
| **[offset](#offset)&nbsp;(&nbsp;`value`&nbsp;`time.Duration`)** | How far back in time to query from the current time  |
| **[period](#period)&nbsp;(&nbsp;`value`&nbsp;`time.Duration`)** | The period or length of time that will be queried from InfluxDB  |
| **[quiet](#quiet)&nbsp;(&nbsp;)** | Suppress errors during execution.  |



### Chaining Methods
[Alert](/kapacitor/v1.4/nodes/query_node/#alert), [Barrier](/kapacitor/v1.4/nodes/query_node/#barrier), [Bottom](/kapacitor/v1.4/nodes/query_node/#bottom), [Combine](/kapacitor/v1.4/nodes/query_node/#combine), [Count](/kapacitor/v1.4/nodes/query_node/#count), [CumulativeSum](/kapacitor/v1.4/nodes/query_node/#cumulativesum), [Deadman](/kapacitor/v1.4/nodes/query_node/#deadman), [Default](/kapacitor/v1.4/nodes/query_node/#default), [Delete](/kapacitor/v1.4/nodes/query_node/#delete), [Derivative](/kapacitor/v1.4/nodes/query_node/#derivative), [Difference](/kapacitor/v1.4/nodes/query_node/#difference), [Distinct](/kapacitor/v1.4/nodes/query_node/#distinct), [Ec2Autoscale](/kapacitor/v1.4/nodes/query_node/#ec2autoscale), [Elapsed](/kapacitor/v1.4/nodes/query_node/#elapsed), [Eval](/kapacitor/v1.4/nodes/query_node/#eval), [First](/kapacitor/v1.4/nodes/query_node/#first), [Flatten](/kapacitor/v1.4/nodes/query_node/#flatten), [HoltWinters](/kapacitor/v1.4/nodes/query_node/#holtwinters), [HoltWintersWithFit](/kapacitor/v1.4/nodes/query_node/#holtwinterswithfit), [HttpOut](/kapacitor/v1.4/nodes/query_node/#httpout), [HttpPost](/kapacitor/v1.4/nodes/query_node/#httppost), [InfluxDBOut](/kapacitor/v1.4/nodes/query_node/#influxdbout), [Join](/kapacitor/v1.4/nodes/query_node/#join), [K8sAutoscale](/kapacitor/v1.4/nodes/query_node/#k8sautoscale), [KapacitorLoopback](/kapacitor/v1.4/nodes/query_node/#kapacitorloopback), [Last](/kapacitor/v1.4/nodes/query_node/#last), [Log](/kapacitor/v1.4/nodes/query_node/#log), [Max](/kapacitor/v1.4/nodes/query_node/#max), [Mean](/kapacitor/v1.4/nodes/query_node/#mean), [Median](/kapacitor/v1.4/nodes/query_node/#median), [Min](/kapacitor/v1.4/nodes/query_node/#min), [Mode](/kapacitor/v1.4/nodes/query_node/#mode), [MovingAverage](/kapacitor/v1.4/nodes/query_node/#movingaverage), [Percentile](/kapacitor/v1.4/nodes/query_node/#percentile), [Sample](/kapacitor/v1.4/nodes/query_node/#sample), [Shift](/kapacitor/v1.4/nodes/query_node/#shift), [Sideload](/kapacitor/v1.4/nodes/query_node/#sideload), [Spread](/kapacitor/v1.4/nodes/query_node/#spread), [StateCount](/kapacitor/v1.4/nodes/query_node/#statecount), [StateDuration](/kapacitor/v1.4/nodes/query_node/#stateduration), [Stats](/kapacitor/v1.4/nodes/query_node/#stats), [Stddev](/kapacitor/v1.4/nodes/query_node/#stddev), [Sum](/kapacitor/v1.4/nodes/query_node/#sum), [SwarmAutoscale](/kapacitor/v1.4/nodes/query_node/#swarmautoscale), [Top](/kapacitor/v1.4/nodes/query_node/#top), [Union](/kapacitor/v1.4/nodes/query_node/#union), [Where](/kapacitor/v1.4/nodes/query_node/#where), [Window](/kapacitor/v1.4/nodes/query_node/#window)

---

### Description

A [QueryNode](/kapacitor/v1.4/nodes/query_node/) defines a source and a schedule for
processing batch data. The data is queried from
an InfluxDB database and then passed into the data pipeline.

Example:


```javascript
 batch
     |query('''
         SELECT mean("value")
         FROM "telegraf"."default".cpu_usage_idle
         WHERE "host" = 'serverA'
     ''')
         .period(1m)
         .every(20s)
         .groupBy(time(10s), 'cpu')
     ...
```

In the above example InfluxDB is queried every 20 seconds; the window of time returned
spans 1 minute and is grouped into 10 second buckets.


<a class="top" href="javascript:document.getElementsByClassName('article-heading')[0].scrollIntoView();" title="top"><span class="icon arrow-up"></span></a>

## Properties

Property methods modify state on the calling node.
They do not add another node to the pipeline, and always return a reference to the calling node.
Property methods are marked using the `.` operator.


### Align

Align start and stop times for quiries with even boundaries of the [QueryNode.Every](/kapacitor/v1.4/nodes/query_node/#every) property.
Does not apply if using the [QueryNode.Cron](/kapacitor/v1.4/nodes/query_node/#cron) property.


```javascript
query.align()
```

<a class="top" href="javascript:document.getElementsByClassName('article-heading')[0].scrollIntoView();" title="top"><span class="icon arrow-up"></span></a>


### AlignGroup

Align the group by time intervals with the start time of the query


```javascript
query.alignGroup()
```

<a class="top" href="javascript:document.getElementsByClassName('article-heading')[0].scrollIntoView();" title="top"><span class="icon arrow-up"></span></a>


### Cluster

The name of a configured InfluxDB cluster.
If empty the default cluster will be used.


```javascript
query.cluster(value string)
```

<a class="top" href="javascript:document.getElementsByClassName('article-heading')[0].scrollIntoView();" title="top"><span class="icon arrow-up"></span></a>


### Cron

Define a schedule using a cron syntax.

The specific cron implementation is documented here:
https://github.com/gorhill/cronexpr#implementation

The Cron property is mutually exclusive with the Every property.


```javascript
query.cron(value string)
```

<a class="top" href="javascript:document.getElementsByClassName('article-heading')[0].scrollIntoView();" title="top"><span class="icon arrow-up"></span></a>


### Every

How often to query InfluxDB.

The Every property is mutually exclusive with the Cron property.


```javascript
query.every(value time.Duration)
```

<a class="top" href="javascript:document.getElementsByClassName('article-heading')[0].scrollIntoView();" title="top"><span class="icon arrow-up"></span></a>


### Fill

Fill the data.
Options are:

- Any numerical value
- null - exhibits the same behavior as the default
- previous - reports the value of the previous window
- none - suppresses timestamps and values where the value is null
- linear - reports the results of linear interpolation


```javascript
query.fill(value interface{})
```

<a class="top" href="javascript:document.getElementsByClassName('article-heading')[0].scrollIntoView();" title="top"><span class="icon arrow-up"></span></a>


### GroupBy

Group the data by a set of dimensions.
Can specify one time dimension.

This property adds a `GROUP BY` clause to the query
so all the normal behaviors when quering InfluxDB with a `GROUP BY` apply.

Use group by time when your period is longer than your group by time interval.

Example:


```javascript
    batch
        |query(...)
            .period(1m)
            .every(1m)
            .groupBy(time(10s), 'tag1', 'tag2'))
            .align()
```

A group by time offset is also possible.

Example:


```javascript
    batch
        |query(...)
            .period(1m)
            .every(1m)
            .groupBy(time(10s, -5s), 'tag1', 'tag2'))
            .align()
            .offset(5s)
```

It is recommended to use [QueryNode.Align](/kapacitor/v1.4/nodes/query_node/#align) and [QueryNode.Offset](/kapacitor/v1.4/nodes/query_node/#offset) in conjunction with
group by time dimensions so that the time bounds match up with the group by intervals.
To automatically align the group by intervals to the start of the query time,
use [QueryNode.AlignGroup.](/kapacitor/v1.4/nodes/query_node/#aligngroup) This is useful in more complex situations, such as when
the groupBy time period is longer than the query frequency.

Example:


```javascript
    batch
        |query(...)
            .period(5m)
            .every(30s)
            .groupBy(time(1m), 'tag1', 'tag2')
            .align()
            .alignGroup()
```

For the above example, without [QueryNode.AlignGroup,](/kapacitor/v1.4/nodes/query_node/#aligngroup) every other query issued by Kapacitor
(at :30 past the minute) will align to :00 seconds instead of the desired :30 seconds,
which would create 6 group by intervals instead of 5, the first and last of which
would only have 30 seconds of data instead of a full minute.
If the group by time offset (i.e. time(t, offset)) is used in conjunction with
[QueryNode.AlignGroup,](/kapacitor/v1.4/nodes/query_node/#aligngroup) the alignment will occur first, and will be offset
the specified amount after.

NOTE: Since [QueryNode.Offset](/kapacitor/v1.4/nodes/query_node/#offset) is inherently a negative property the second "offset" argument to the "time" function is negative to match.



```javascript
query.groupBy(d ...interface{})
```

<a class="top" href="javascript:document.getElementsByClassName('article-heading')[0].scrollIntoView();" title="top"><span class="icon arrow-up"></span></a>


### GroupByMeasurement

If set will include the measurement name in the group ID.
Along with any other group by dimensions.

Example:


```javascript
 batch
      |query('SELECT sum("value") FROM "telegraf"."autogen"./process_.*/')
          .groupByMeasurement()
          .groupBy('host')
```

The above example selects data from several measurements matching `/process_.*/ and
then each point is grouped by the host tag and measurement name.
Thus keeping measurements in their own groups.


```javascript
query.groupByMeasurement()
```

<a class="top" href="javascript:document.getElementsByClassName('article-heading')[0].scrollIntoView();" title="top"><span class="icon arrow-up"></span></a>


### Offset

How far back in time to query from the current time

For example an Offest of 2 hours and an Every of 5m,
Kapacitor will query InfluxDB every 5 minutes for the window of data 2 hours ago.

This applies to Cron schedules as well. If the cron specifies to run every Sunday at
1 AM and the Offset is 1 hour. Then at 1 AM on Sunday the data from 12 AM will be queried.


```javascript
query.offset(value time.Duration)
```

<a class="top" href="javascript:document.getElementsByClassName('article-heading')[0].scrollIntoView();" title="top"><span class="icon arrow-up"></span></a>


### Period

The period or length of time that will be queried from InfluxDB


```javascript
query.period(value time.Duration)
```

<a class="top" href="javascript:document.getElementsByClassName('article-heading')[0].scrollIntoView();" title="top"><span class="icon arrow-up"></span></a>


### Quiet

Suppress errors during execution.

```javascript
query.quiet()
```

<a class="top" href="javascript:document.getElementsByClassName('article-heading')[0].scrollIntoView();" title="top"><span class="icon arrow-up"></span></a>


## Chaining Methods

Chaining methods create a new node in the pipeline as a child of the calling node.
They do not modify the calling node.
Chaining methods are marked using the `|` operator.


### Alert

Create an alert node, which can trigger alerts.


```javascript
query|alert()
```

Returns: [AlertNode](/kapacitor/v1.4/nodes/alert_node/)

<a class="top" href="javascript:document.getElementsByClassName('article-heading')[0].scrollIntoView();" title="top"><span class="icon arrow-up"></span></a>

### Barrier

Create a new Barrier node that emits a BarrierMessage periodically

One BarrierMessage will be emitted every period duration


```javascript
query|barrier()
```

Returns: [BarrierNode](/kapacitor/v1.4/nodes/barrier_node/)

<a class="top" href="javascript:document.getElementsByClassName('article-heading')[0].scrollIntoView();" title="top"><span class="icon arrow-up"></span></a>

### Bottom

Select the bottom `num` points for `field` and sort by any extra tags or fields.


```javascript
query|bottom(num int64, field string, fieldsAndTags ...string)
```

Returns: [InfluxQLNode](/kapacitor/v1.4/nodes/influx_q_l_node/)

<a class="top" href="javascript:document.getElementsByClassName('article-heading')[0].scrollIntoView();" title="top"><span class="icon arrow-up"></span></a>

### Combine

Combine this node with itself. The data are combined on timestamp.


```javascript
query|combine(expressions ...ast.LambdaNode)
```

Returns: [CombineNode](/kapacitor/v1.4/nodes/combine_node/)

<a class="top" href="javascript:document.getElementsByClassName('article-heading')[0].scrollIntoView();" title="top"><span class="icon arrow-up"></span></a>

### Count

Count the number of points.


```javascript
query|count(field string)
```

Returns: [InfluxQLNode](/kapacitor/v1.4/nodes/influx_q_l_node/)

<a class="top" href="javascript:document.getElementsByClassName('article-heading')[0].scrollIntoView();" title="top"><span class="icon arrow-up"></span></a>

### CumulativeSum

Compute a cumulative sum of each point that is received.
A point is emitted for every point collected.


```javascript
query|cumulativeSum(field string)
```

Returns: [InfluxQLNode](/kapacitor/v1.4/nodes/influx_q_l_node/)

<a class="top" href="javascript:document.getElementsByClassName('article-heading')[0].scrollIntoView();" title="top"><span class="icon arrow-up"></span></a>

### Deadman

Helper function for creating an alert on low throughput, a.k.a. deadman's switch.

- Threshold -- trigger alert if throughput drops below threshold in points/interval.
- Interval -- how often to check the throughput.
- Expressions -- optional list of expressions to also evaluate. Useful for time of day alerting.

Example:


```javascript
    var data = stream
        |from()...
    // Trigger critical alert if the throughput drops below 100 points per 10s and checked every 10s.
    data
        |deadman(100.0, 10s)
    //Do normal processing of data
    data...
```

The above is equivalent to this
Example:


```javascript
    var data = stream
        |from()...
    // Trigger critical alert if the throughput drops below 100 points per 10s and checked every 10s.
    data
        |stats(10s)
            .align()
        |derivative('emitted')
            .unit(10s)
            .nonNegative()
        |alert()
            .id('node \'stream0\' in task \'{{ .TaskName }}\'')
            .message('{{ .ID }} is {{ if eq .Level "OK" }}alive{{ else }}dead{{ end }}: {{ index .Fields "emitted" | printf "%0.3f" }} points/10s.')
            .crit(lambda: "emitted" <= 100.0)
    //Do normal processing of data
    data...
```

The `id` and `message` alert properties can be configured globally via the 'deadman' configuration section.

Since the [AlertNode](/kapacitor/v1.4/nodes/alert_node/) is the last piece it can be further modified as usual.
Example:


```javascript
    var data = stream
        |from()...
    // Trigger critical alert if the throughput drops below 100 points per 10s and checked every 10s.
    data
        |deadman(100.0, 10s)
            .slack()
            .channel('#dead_tasks')
    //Do normal processing of data
    data...
```

You can specify additional lambda expressions to further constrain when the deadman's switch is triggered.
Example:


```javascript
    var data = stream
        |from()...
    // Trigger critical alert if the throughput drops below 100 points per 10s and checked every 10s.
    // Only trigger the alert if the time of day is between 8am-5pm.
    data
        |deadman(100.0, 10s, lambda: hour("time") >= 8 AND hour("time") <= 17)
    //Do normal processing of data
    data...
```



```javascript
query|deadman(threshold float64, interval time.Duration, expr ...ast.LambdaNode)
```

Returns: [AlertNode](/kapacitor/v1.4/nodes/alert_node/)

<a class="top" href="javascript:document.getElementsByClassName('article-heading')[0].scrollIntoView();" title="top"><span class="icon arrow-up"></span></a>

### Default

Create a node that can set defaults for missing tags or fields.


```javascript
query|default()
```

Returns: [DefaultNode](/kapacitor/v1.4/nodes/default_node/)

<a class="top" href="javascript:document.getElementsByClassName('article-heading')[0].scrollIntoView();" title="top"><span class="icon arrow-up"></span></a>

### Delete

Create a node that can delete tags or fields.


```javascript
query|delete()
```

Returns: [DeleteNode](/kapacitor/v1.4/nodes/delete_node/)

<a class="top" href="javascript:document.getElementsByClassName('article-heading')[0].scrollIntoView();" title="top"><span class="icon arrow-up"></span></a>

### Derivative

Create a new node that computes the derivative of adjacent points.


```javascript
query|derivative(field string)
```

Returns: [DerivativeNode](/kapacitor/v1.4/nodes/derivative_node/)

<a class="top" href="javascript:document.getElementsByClassName('article-heading')[0].scrollIntoView();" title="top"><span class="icon arrow-up"></span></a>

### Difference

Compute the difference between points independent of elapsed time.


```javascript
query|difference(field string)
```

Returns: [InfluxQLNode](/kapacitor/v1.4/nodes/influx_q_l_node/)

<a class="top" href="javascript:document.getElementsByClassName('article-heading')[0].scrollIntoView();" title="top"><span class="icon arrow-up"></span></a>

### Distinct

Produce batch of only the distinct points.


```javascript
query|distinct(field string)
```

Returns: [InfluxQLNode](/kapacitor/v1.4/nodes/influx_q_l_node/)

<a class="top" href="javascript:document.getElementsByClassName('article-heading')[0].scrollIntoView();" title="top"><span class="icon arrow-up"></span></a>

### Ec2Autoscale

Create a node that can trigger autoscale events for a ec2 autoscalegroup.


```javascript
query|ec2Autoscale()
```

Returns: [Ec2AutoscaleNode](/kapacitor/v1.4/nodes/ec2_autoscale_node/)

<a class="top" href="javascript:document.getElementsByClassName('article-heading')[0].scrollIntoView();" title="top"><span class="icon arrow-up"></span></a>

### Elapsed

Compute the elapsed time between points


```javascript
query|elapsed(field string, unit time.Duration)
```

Returns: [InfluxQLNode](/kapacitor/v1.4/nodes/influx_q_l_node/)

<a class="top" href="javascript:document.getElementsByClassName('article-heading')[0].scrollIntoView();" title="top"><span class="icon arrow-up"></span></a>

### Eval

Create an eval node that will evaluate the given transformation function to each data point.
A list of expressions may be provided and will be evaluated in the order they are given.
The results are available to later expressions.


```javascript
query|eval(expressions ...ast.LambdaNode)
```

Returns: [EvalNode](/kapacitor/v1.4/nodes/eval_node/)

<a class="top" href="javascript:document.getElementsByClassName('article-heading')[0].scrollIntoView();" title="top"><span class="icon arrow-up"></span></a>

### First

Select the first point.


```javascript
query|first(field string)
```

Returns: [InfluxQLNode](/kapacitor/v1.4/nodes/influx_q_l_node/)

<a class="top" href="javascript:document.getElementsByClassName('article-heading')[0].scrollIntoView();" title="top"><span class="icon arrow-up"></span></a>

### Flatten

Flatten points with similar times into a single point.


```javascript
query|flatten()
```

Returns: [FlattenNode](/kapacitor/v1.4/nodes/flatten_node/)

<a class="top" href="javascript:document.getElementsByClassName('article-heading')[0].scrollIntoView();" title="top"><span class="icon arrow-up"></span></a>

### HoltWinters

Compute the holt-winters (https://docs.influxdata.com/influxdb/latest/query_language/functions/#holt-winters) forecast of a data set.


```javascript
query|holtWinters(field string, h int64, m int64, interval time.Duration)
```

Returns: [InfluxQLNode](/kapacitor/v1.4/nodes/influx_q_l_node/)

<a class="top" href="javascript:document.getElementsByClassName('article-heading')[0].scrollIntoView();" title="top"><span class="icon arrow-up"></span></a>

### HoltWintersWithFit

Compute the holt-winters (https://docs.influxdata.com/influxdb/latest/query_language/functions/#holt-winters) forecast of a data set.
This method also outputs all the points used to fit the data in addition to the forecasted data.


```javascript
query|holtWintersWithFit(field string, h int64, m int64, interval time.Duration)
```

Returns: [InfluxQLNode](/kapacitor/v1.4/nodes/influx_q_l_node/)

<a class="top" href="javascript:document.getElementsByClassName('article-heading')[0].scrollIntoView();" title="top"><span class="icon arrow-up"></span></a>

### HttpOut

Create an HTTP output node that caches the most recent data it has received.
The cached data are available at the given endpoint.
The endpoint is the relative path from the API endpoint of the running task.
For example, if the task endpoint is at `/kapacitor/v1/tasks/<task_id>` and endpoint is
`top10`, then the data can be requested from `/kapacitor/v1/tasks/<task_id>/top10`.


```javascript
query|httpOut(endpoint string)
```

Returns: [HTTPOutNode](/kapacitor/v1.4/nodes/http_out_node/)

<a class="top" href="javascript:document.getElementsByClassName('article-heading')[0].scrollIntoView();" title="top"><span class="icon arrow-up"></span></a>

### HttpPost

Creates an HTTP Post node that POSTS received data to the provided HTTP endpoint.
HttpPost expects 0 or 1 arguments. If 0 arguments are provided, you must specify an
endpoint property method.


```javascript
query|httpPost(url ...string)
```

Returns: [HTTPPostNode](/kapacitor/v1.4/nodes/http_post_node/)

<a class="top" href="javascript:document.getElementsByClassName('article-heading')[0].scrollIntoView();" title="top"><span class="icon arrow-up"></span></a>

### InfluxDBOut

Create an influxdb output node that will store the incoming data into InfluxDB.


```javascript
query|influxDBOut()
```

Returns: [InfluxDBOutNode](/kapacitor/v1.4/nodes/influx_d_b_out_node/)

<a class="top" href="javascript:document.getElementsByClassName('article-heading')[0].scrollIntoView();" title="top"><span class="icon arrow-up"></span></a>

### Join

Join this node with other nodes. The data are joined on timestamp.


```javascript
query|join(others ...Node)
```

Returns: [JoinNode](/kapacitor/v1.4/nodes/join_node/)

<a class="top" href="javascript:document.getElementsByClassName('article-heading')[0].scrollIntoView();" title="top"><span class="icon arrow-up"></span></a>

### K8sAutoscale

Create a node that can trigger autoscale events for a kubernetes cluster.


```javascript
query|k8sAutoscale()
```

Returns: [K8sAutoscaleNode](/kapacitor/v1.4/nodes/k8s_autoscale_node/)

<a class="top" href="javascript:document.getElementsByClassName('article-heading')[0].scrollIntoView();" title="top"><span class="icon arrow-up"></span></a>

### KapacitorLoopback

Create an kapacitor loopback node that will send data back into Kapacitor as a stream.


```javascript
query|kapacitorLoopback()
```

Returns: [KapacitorLoopbackNode](/kapacitor/v1.4/nodes/kapacitor_loopback_node/)

<a class="top" href="javascript:document.getElementsByClassName('article-heading')[0].scrollIntoView();" title="top"><span class="icon arrow-up"></span></a>

### Last

Select the last point.


```javascript
query|last(field string)
```

Returns: [InfluxQLNode](/kapacitor/v1.4/nodes/influx_q_l_node/)

<a class="top" href="javascript:document.getElementsByClassName('article-heading')[0].scrollIntoView();" title="top"><span class="icon arrow-up"></span></a>

### Log

Create a node that logs all data it receives.


```javascript
query|log()
```

Returns: [LogNode](/kapacitor/v1.4/nodes/log_node/)

<a class="top" href="javascript:document.getElementsByClassName('article-heading')[0].scrollIntoView();" title="top"><span class="icon arrow-up"></span></a>

### Max

Select the maximum point.


```javascript
query|max(field string)
```

Returns: [InfluxQLNode](/kapacitor/v1.4/nodes/influx_q_l_node/)

<a class="top" href="javascript:document.getElementsByClassName('article-heading')[0].scrollIntoView();" title="top"><span class="icon arrow-up"></span></a>

### Mean

Compute the mean of the data.


```javascript
query|mean(field string)
```

Returns: [InfluxQLNode](/kapacitor/v1.4/nodes/influx_q_l_node/)

<a class="top" href="javascript:document.getElementsByClassName('article-heading')[0].scrollIntoView();" title="top"><span class="icon arrow-up"></span></a>

### Median

Compute the median of the data. Note, this method is not a selector,
if you want the median point use `.percentile(field, 50.0)`.


```javascript
query|median(field string)
```

Returns: [InfluxQLNode](/kapacitor/v1.4/nodes/influx_q_l_node/)

<a class="top" href="javascript:document.getElementsByClassName('article-heading')[0].scrollIntoView();" title="top"><span class="icon arrow-up"></span></a>

### Min

Select the minimum point.


```javascript
query|min(field string)
```

Returns: [InfluxQLNode](/kapacitor/v1.4/nodes/influx_q_l_node/)

<a class="top" href="javascript:document.getElementsByClassName('article-heading')[0].scrollIntoView();" title="top"><span class="icon arrow-up"></span></a>

### Mode

Compute the mode of the data.


```javascript
query|mode(field string)
```

Returns: [InfluxQLNode](/kapacitor/v1.4/nodes/influx_q_l_node/)

<a class="top" href="javascript:document.getElementsByClassName('article-heading')[0].scrollIntoView();" title="top"><span class="icon arrow-up"></span></a>

### MovingAverage

Compute a moving average of the last window points.
No points are emitted until the window is full.


```javascript
query|movingAverage(field string, window int64)
```

Returns: [InfluxQLNode](/kapacitor/v1.4/nodes/influx_q_l_node/)

<a class="top" href="javascript:document.getElementsByClassName('article-heading')[0].scrollIntoView();" title="top"><span class="icon arrow-up"></span></a>

### Percentile

Select a point at the given percentile. This is a selector function, no interpolation between points is performed.


```javascript
query|percentile(field string, percentile float64)
```

Returns: [InfluxQLNode](/kapacitor/v1.4/nodes/influx_q_l_node/)

<a class="top" href="javascript:document.getElementsByClassName('article-heading')[0].scrollIntoView();" title="top"><span class="icon arrow-up"></span></a>

### Sample

Create a new node that samples the incoming points or batches.

One point will be emitted every count or duration specified.


```javascript
query|sample(rate interface{})
```

Returns: [SampleNode](/kapacitor/v1.4/nodes/sample_node/)

<a class="top" href="javascript:document.getElementsByClassName('article-heading')[0].scrollIntoView();" title="top"><span class="icon arrow-up"></span></a>

### Shift

Create a new node that shifts the incoming points or batches in time.


```javascript
query|shift(shift time.Duration)
```

Returns: [ShiftNode](/kapacitor/v1.4/nodes/shift_node/)

<a class="top" href="javascript:document.getElementsByClassName('article-heading')[0].scrollIntoView();" title="top"><span class="icon arrow-up"></span></a>

### Sideload

Create a node that can load data from external sources


```javascript
query|sideload()
```

Returns: [SideloadNode](/kapacitor/v1.4/nodes/sideload_node/)

<a class="top" href="javascript:document.getElementsByClassName('article-heading')[0].scrollIntoView();" title="top"><span class="icon arrow-up"></span></a>

### Spread

Compute the difference between `min` and `max` points.


```javascript
query|spread(field string)
```

Returns: [InfluxQLNode](/kapacitor/v1.4/nodes/influx_q_l_node/)

<a class="top" href="javascript:document.getElementsByClassName('article-heading')[0].scrollIntoView();" title="top"><span class="icon arrow-up"></span></a>

### StateCount

Create a node that tracks number of consecutive points in a given state.


```javascript
query|stateCount(expression ast.LambdaNode)
```

Returns: [StateCountNode](/kapacitor/v1.4/nodes/state_count_node/)

<a class="top" href="javascript:document.getElementsByClassName('article-heading')[0].scrollIntoView();" title="top"><span class="icon arrow-up"></span></a>

### StateDuration

Create a node that tracks duration in a given state.


```javascript
query|stateDuration(expression ast.LambdaNode)
```

Returns: [StateDurationNode](/kapacitor/v1.4/nodes/state_duration_node/)

<a class="top" href="javascript:document.getElementsByClassName('article-heading')[0].scrollIntoView();" title="top"><span class="icon arrow-up"></span></a>

### Stats

Create a new stream of data that contains the internal statistics of the node.
The interval represents how often to emit the statistics based on real time.
This means the interval time is independent of the times of the data points the source node is receiving.


```javascript
query|stats(interval time.Duration)
```

Returns: [StatsNode](/kapacitor/v1.4/nodes/stats_node/)

<a class="top" href="javascript:document.getElementsByClassName('article-heading')[0].scrollIntoView();" title="top"><span class="icon arrow-up"></span></a>

### Stddev

Compute the standard deviation.


```javascript
query|stddev(field string)
```

Returns: [InfluxQLNode](/kapacitor/v1.4/nodes/influx_q_l_node/)

<a class="top" href="javascript:document.getElementsByClassName('article-heading')[0].scrollIntoView();" title="top"><span class="icon arrow-up"></span></a>

### Sum

Compute the sum of all values.


```javascript
query|sum(field string)
```

Returns: [InfluxQLNode](/kapacitor/v1.4/nodes/influx_q_l_node/)

<a class="top" href="javascript:document.getElementsByClassName('article-heading')[0].scrollIntoView();" title="top"><span class="icon arrow-up"></span></a>

### SwarmAutoscale

Create a node that can trigger autoscale events for a docker swarm cluster.


```javascript
query|swarmAutoscale()
```

Returns: [SwarmAutoscaleNode](/kapacitor/v1.4/nodes/swarm_autoscale_node/)

<a class="top" href="javascript:document.getElementsByClassName('article-heading')[0].scrollIntoView();" title="top"><span class="icon arrow-up"></span></a>

### Top

Select the top `num` points for `field` and sort by any extra tags or fields.


```javascript
query|top(num int64, field string, fieldsAndTags ...string)
```

Returns: [InfluxQLNode](/kapacitor/v1.4/nodes/influx_q_l_node/)

<a class="top" href="javascript:document.getElementsByClassName('article-heading')[0].scrollIntoView();" title="top"><span class="icon arrow-up"></span></a>

### Union

Perform the union of this node and all other given nodes.


```javascript
query|union(node ...Node)
```

Returns: [UnionNode](/kapacitor/v1.4/nodes/union_node/)

<a class="top" href="javascript:document.getElementsByClassName('article-heading')[0].scrollIntoView();" title="top"><span class="icon arrow-up"></span></a>

### Where

Create a new node that filters the data stream by a given expression.


```javascript
query|where(expression ast.LambdaNode)
```

Returns: [WhereNode](/kapacitor/v1.4/nodes/where_node/)

<a class="top" href="javascript:document.getElementsByClassName('article-heading')[0].scrollIntoView();" title="top"><span class="icon arrow-up"></span></a>

### Window

Create a new node that windows the stream by time.

NOTE: Window can only be applied to stream edges.


```javascript
query|window()
```

Returns: [WindowNode](/kapacitor/v1.4/nodes/window_node/)

<a class="top" href="javascript:document.getElementsByClassName('article-heading')[0].scrollIntoView();" title="top"><span class="icon arrow-up"></span></a>