---
description: How to instrument your service using Aspecto
---

# Install

## Install the Aspecto SDK for node.js

Aspecto uses [OpenTelemetry](www.opentelemetry.io) to collect data. 

Install our Node.js library using `npm`:

```bash
$ npm install @aspecto/opentelemetry
```

### Usage

In the root folder create an `aspecto.json` file with content `{"token" : "-- token goes here --"}`.   
Alternatively, you can provide it through ASPECTO\_AUTH env param OR `aspectoAuth`init option.  
Obtain your token [here](https://app.aspecto.io/app/integration/api-key).

Add this call at the top of your app entry point:

```javascript
require('@aspecto/opentelemetry')();
```

### Configuration

You can pass the following configuration to the Aspecto client:

<table>
  <thead>
    <tr>
      <th style="text-align:left">Option</th>
      <th style="text-align:left">Type</th>
      <th style="text-align:left">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left"><code>env</code>
      </td>
      <td style="text-align:left">string</td>
      <td style="text-align:left">set environment name manually instead of using <code>NODE_ENV</code> environment
        variable</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>aspectoAuth</code>
      </td>
      <td style="text-align:left">UUID</td>
      <td style="text-align:left">set Aspecto token from code instead of using <code>aspecto.json</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>packageName</code>
      </td>
      <td style="text-align:left">string</td>
      <td style="text-align:left">set packageName manually instead of reading it from <code>package.json</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>packageVersion</code>
      </td>
      <td style="text-align:left">string</td>
      <td style="text-align:left">set packageVersion manually instead of reading it from <code>package.json</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>local</code>
      </td>
      <td style="text-align:left">boolean</td>
      <td style="text-align:left">when set to true, enable <a href="https://www.npmjs.com/package/@aspecto/opentelemetry#live-flows">live flows</a>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>isolate</code>
      </td>
      <td style="text-align:left">boolean</td>
      <td style="text-align:left">when set to true, enable isolated mode for <a href="https://www.npmjs.com/package/@aspecto/opentelemetry#live-flows">live flows</a>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>liveExporterPort</code>
      </td>
      <td style="text-align:left">number</td>
      <td style="text-align:left">specify port for <a href="https://www.npmjs.com/package/@aspecto/opentelemetry#live-flows">live flows</a>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>logger</code>
      </td>
      <td style="text-align:left">logger interface</td>
      <td style="text-align:left">logger to be used in this tracing library. common use for debugging <code>logger: console</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>customZipkinEndpoint</code>
      </td>
      <td style="text-align:left">URL</td>
      <td style="text-align:left">Send all traces to additional Zipkin server for debug</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>samplingRatio</code>
      </td>
      <td style="text-align:left">number</td>
      <td style="text-align:left">
        <p>Rate of traces to be sampled. Between 0 to 1 (default is 1).</p>
        <p>Uses <a href="https://github.com/open-telemetry/opentelemetry-specification/blob/master/specification/trace/sdk.md#parentbased">parent-based</a> sampling.</p>
      </td>
    </tr>
  </tbody>
</table>

### Live Flows

{% page-ref page="live-debugging/live-flows-overview.md" %}

Live Flows captures all payloads and traces in your **local environment** and automatically extracts the topology & dependencies between endpoints. 

Activate it using `{local:true}`, like this:

```javascript
require('@aspecto/opentelemetry')({ local: true });
```

Live flows works in two modes:

* connected
* isolated

#### Connected mode

This is the default mode. It allows you to capture flows from all the microservices that you're running locally \(both on the host env and docker\) with`local`mode enabled. Once the process starts it will output the following link:

```text
=====================================================================================================================================
|                                                                                                                                   |
| 🕵️‍♀️See the live tracing stream at https://app.aspecto.io/app/live-flows/sessions?instanceId=14243e72-14dc-4255-87af-ef846b247578   |
|                                                                                                                                   |
=====================================================================================================================================
```

Click on the link to open the Live Flow, to see traces from all the microservices that are running on your environment that have local mode enabled. The link is valid for a limited period of time \(a couple of days, but it may change in the future\).   
If you don't see a trace from some microservice \(or none of them\), click the newly-generated link.

#### Isolated mode

In this mode, you can only see flows from one microservice. Also, in this case, all the data is being sent directly to the browser. To activate isolated mode use `isolate` option like so:

```javascript
require('@aspecto/opentelemetry')({ local: true, isolate: true });
```

In isolated mode, the message in the console will look like this \(with `port` parameter\):

```text
===============================================================================================
|                                                                                             |
| 🕵️‍♀️ See the live tracing stream at https://app.aspecto.io/app/live-flows/sessions?port=59778 |
|                                                                                             |
===============================================================================================
```

Note: If the Live Flows port is not static, set the `liveExporterPort` configuration option.  


