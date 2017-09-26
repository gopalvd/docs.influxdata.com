---
title: TICKscript Language Introduction

menu:
  kapacitor_1_3:
    name: Introduction
    identifier: tick_intro
    parent: tick
    weight: 1
---
_N.B. the following is a proposed rewrite of the current [introduction to TICKscript](https://docs.influxdata.com/kapacitor/v1.3/tick/)_

Kapacitor uses a Domain Specific Language(DSL) named **TICKscript**.  TICKscript is used in `.tick` files to define **pipelines** for processing data in Kapacitor.  The TICKscript language is designed to chain together the invocation of data processing operations defined in **nodes**.  

Nodes
-----

In TICKscript the fundamental type is the processing **node**.  A processing node has **properties** and **chaining methods**.  A new processing node can be instantiated from a parent node using a chaining method of that parent node.  For each **node type** the signature of this method will be the same, regardless of the parent node type.  The chaining method can accept zero or more arguments used to initialize internal properties of the new node instance.  The most common argument types used during instantiation are:

   * Strings representing usually a query expression, a measurement name, a data series tag or field.
   * **lambda expressions** representing small function blocks to be applied to the data.
   * GO time constructs.
   * Other standard types from the GO language.     

The top level nodes that establish the processing style&mdash;`stream` and `batch`&mdash;are simply declared and take no arguments.  

Each script has a flat scope and each variable in the scope references a node instance with methods that can then be called.

These methods come in two forms.

* **Property methods** &ndash; A property method modifies the internal properties of a node and returns a reference to the same node.  Property methods are called using dot ('.') notation.
* **Chaining methods** &ndash; A chaining method creates a new child node and returns a reference to it.  Chaining methods are called using pipe ('|') notation.

The reference documentation lists each node's property and chaining methods along with examples and descriptions.

<!-- The `stream` and `batch` variables are an instance of a [StreamNode](/kapacitor/v1.3/nodes/stream_node/) or [BatchNode](/kapacitor/v1.3/nodes/batch_node/) respectively. -->

Pipelines
---------
Every TICKscript is broken into one or more **pipelines**.  A pipeline and its top node can be assigned to a variable allowing the results of different pipelines to be combined.  Alternately a simple TICKscript can use only one anonymous pipeline.  The pipeline processing style is set as either `stream` or `batch`.  These two types of pipelines cannot be combined.

<!-- Kapacitor uses TICKscripts to define data processing pipelines.
A pipeline is set of nodes that process data and edges that connect the nodes. -->
Pipelines in Kapacitor are directed acyclic graphs ([DAGs](https://en.wikipedia.org/wiki/Directed_acyclic_graph)).  This means that
each edge has a direction down which data flows, and that there cannot be any cycles in the pipeline.  An edge can also be thought of as the data-flow relationship that exists between a parent node and its child or between a child and its sibling.  

<!-- Each edge has a type, one of the following: -->

At the start of any pipeline will be declared one of two fundamental edges.  This first edge establishes the style of processing for all edges that follow.

* `stream`&rarr;`from()` &ndash; an edge that transfers data a single data point at a time.
* `batch`&rarr;`query()` &ndash; an edge that transfers data in chunks instead of one point at a time.  

When connecting nodes and then creating a new Kapacitor task, Kapacitor will check whether or not the TICKscript syntax is well formed, however it will not inform the user of the existence of edges connecting incompatible node types.  Rather all edges will be validated at runtime.

Be aware that a syntactically correct script may define a pipeline that is invalid.

Examples
--------

**Example 1 &ndash; An elementary stream &rarr; from() pipeline**
```javascript
stream
    |from()
        .measurement('cpu')
    |httpOut('dump')
```

When used to create a task called `sf_task` with the default Telegraf database, the TICKscript in Example 1 will simply dump the latest cpu datapoint as JSON to the HTTP REST endpoint(e.g http://localhost:9092/kapacitor/v1/tasks/sf_task/dump).  This example contains three nodes:

   * The base `stream` node.
   * The requisite `from()` node, that defines the stream of data points.
   * The processing node `httpOut()`, that defines one step in processing the data.  In this case it is to publish it to Kapacitor's REST service.  

It contains two edges.

   * `stream` &rarr; `from()` &ndash; sets the processing style and the data stream.
   * `from()` &rarr; `httpOut()` &ndash; passes the data stream to the HTTP output processing node.

It contains one property method, which is the call on the `from()` node to `.measurement('cpu')` defining the measurement to be used for further processing.  

**Example 2 &ndash; An elementary batch &rarr; query() pipeline**

```javascript
batch
    |query('SELECT * FROM "telegraf"."autogen".cpu WHERE time > now() - 10s')
        .period(10s)
        .every(10s)
    |httpOut('dump')
```

When used to create a task called `bq_task` with the default Telegraf database, the TICKscript in Example 2 will simply dump the last cpu datapoint of the batch of measurements representing the last 10 seconds of activity to the HTTP REST endpoint(e.g. http://localhost:9092/kapacitor/v1/tasks/bq_task/dump). This example contains three nodes:

   * The base `batch` node.
   * The requisite `query()` node, that defines the data set.
   * The processing node `httpOut()`, that defines the one step in processing the data set.  In this case it is to publish it to Kapacitor's REST service.

It contains two edges.

   * `batch` &rarr; `query()` &ndash; sets the processing style and data set.
   * `query()` &rarr; `httpOut()` &ndash; passes the data set to the HTTP output processing node.

It contains two property methods, which are called from the `query()` node.    

   * `period()` &ndash; sets the period in time which the batch of data will cover.
   * `every()` &ndash; sets the frequency at which the batch of data will be processed.


Example (original)
-------

```javascript
stream
    |from()
        .measurement('app')
    |eval(lambda: "errors" / "total")
        .as('error_percent')
    // Write the transformed data to InfluxDB
    |influxDBOut()
        .database('mydb')
        .retentionPolicy('myrp')
        .measurement('errors')
        .tag('kapacitor', 'true')
        .tag('version', '0.2')
```

<!-- TODO - get correct link -->
The next section covers [TICKscript syntax](/kapacitor/v1.3/tick/syntax/) in more detail. [Continue...](/kapacitor/v1.3/tick/syntax/)