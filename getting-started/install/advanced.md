---
description: Advanced configuration
---

# Advanced

### Configuration

You can pass a configuration object when initializing Aspecto, like this for example:

```javascript
require('@aspecto/opentelemetry')({
    packageName: 'my-package',
    env: 'production',
    samplingRatio: 0.1
});
```

  
Available install configurations \(all are optional\):

<table>
  <thead>
    <tr>
      <th style="text-align:left">Option</th>
      <th style="text-align:left">Type</th>
      <th style="text-align:left">Description</th>
      <th style="text-align:left">Default</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left"><code>env</code>
      </td>
      <td style="text-align:left">string</td>
      <td style="text-align:left">Set environment name manually</td>
      <td style="text-align:left"><code>process.env.NODE_ENV</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>aspectoAuth</code>
      </td>
      <td style="text-align:left">UUID</td>
      <td style="text-align:left">Set Aspecto token from code instead of using <code>aspecto.json</code>
      </td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left"><code>packageName</code>
      </td>
      <td style="text-align:left">string</td>
      <td style="text-align:left">Service name</td>
      <td style="text-align:left">&quot;name&quot; in <code>package.json</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>packageVersion</code>
      </td>
      <td style="text-align:left">string</td>
      <td style="text-align:left">Service version</td>
      <td style="text-align:left">&quot;version&quot; is <code>package.json</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>local</code>
      </td>
      <td style="text-align:left">boolean</td>
      <td style="text-align:left">When set to true, enable <a href="https://www.npmjs.com/package/@aspecto/opentelemetry#live-flows">live flows</a>
      </td>
      <td style="text-align:left">false</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>liveExporterPort</code>
      </td>
      <td style="text-align:left">number</td>
      <td style="text-align:left">Specify port for <a href="https://www.npmjs.com/package/@aspecto/opentelemetry#live-flows">live flows</a>
      </td>
      <td style="text-align:left">random</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>logger</code>
      </td>
      <td style="text-align:left">logger interface</td>
      <td style="text-align:left">Logger to be used in this tracing library. common use for debugging <code>logger: console</code>
      </td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left"><code>customZipkinEndpoint</code>
      </td>
      <td style="text-align:left">URL</td>
      <td style="text-align:left">Send all traces to additional Zipkin server for debug</td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left"><code>samplingRatio</code>
      </td>
      <td style="text-align:left">number</td>
      <td style="text-align:left">
        <p>Rate of traces to be sampled. Between 0 to 1.</p>
        <p>Uses <a href="https://github.com/open-telemetry/opentelemetry-specification/blob/master/specification/trace/sdk.md#parentbased">parent-based</a> sampling.</p>
      </td>
      <td style="text-align:left">1</td>
    </tr>
  </tbody>
</table>

## Advanced Settings

#### Exporting Format

Data collected by the SDK is sent to Aspecto's collector in **protobuf** format by default.   
An alternative is to send the data in JSON format, which uses less CPU for encoding the message but is more verbose - causing more data to be sent over the network.   
To export data over JSON instead of protobuf, set the environment variable  `ASPECTO_TRACE_EXPORT_JSON` to any value.

#### Disabling Aspecto

Set the environment variable `DISABLE_ASPECTO` to any value, to disable Aspecto.  
Affect lambda and GCF wrappers as well.  
Useful when running unit tests, or as a simple kill switch.

####  **Live Flow**

Live flow allows you to capture flows from all the microservices that you're running locally \(both on the host env and docker\) with`local`mode enabled. To activate live flow mode use `local` option like so:

```javascript
require('@aspecto/opentelemetry')({ local: true });
```

 Once the process starts it will output the following link:

```text
=====================================================================================================================================
|                                                                                                                                   |
| 🕵️‍♀️See the live tracing stream at https://app.aspecto.io/app/live-flows/sessions?instanceId=14243e72-14dc-4255-87af-ef846b247578   |
|                                                                                                                                   |
=====================================================================================================================================
```

Click on the link to open the Live Flow, to see traces from all the microservices that are running on your environment that have local mode enabled. The link is valid for a limited period of time \(a couple of days, but it may change in the future\).   
If you don't see a trace from some microservice \(or none of them\), click the newly-generated link.

## 

