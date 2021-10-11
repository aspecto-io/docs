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

\
Available install configurations (all are optional):

| Option                 | Type             | Description                                                                                                                                                                                                           | Default                     |
| ---------------------- | ---------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------- |
| `env`                  | string           | Set environment name manually                                                                                                                                                                                         | `process.env.NODE_ENV`      |
| `aspectoAuth`          | UUID             | Set Aspecto token from code instead of using `aspecto.json`                                                                                                                                                           |                             |
| `packageName`          | string           | Service name                                                                                                                                                                                                          | "name" in `package.json`    |
| `packageVersion`       | string           | Service version                                                                                                                                                                                                       | "version" is `package.json` |
| `local`                | boolean          | When set to true, enable [live flows](https://www.npmjs.com/package/@aspecto/opentelemetry#live-flows)                                                                                                                | false                       |
| `liveExporterPort`     | number           | Specify port for [live flows](https://www.npmjs.com/package/@aspecto/opentelemetry#live-flows)                                                                                                                        | random                      |
| `logger`               | logger interface | Logger to be used in this tracing library. common use for debugging `logger: console`                                                                                                                                 |                             |
| `customZipkinEndpoint` | URL              | Send all traces to additional Zipkin server for debug                                                                                                                                                                 |                             |
| `samplingRatio`        | number           | <p>Rate of traces to be sampled. Between 0 to 1.</p><p>Uses <a href="https://github.com/open-telemetry/opentelemetry-specification/blob/master/specification/trace/sdk.md#parentbased">parent-based</a> sampling.</p> | 1                           |
| `collectPayloads`      | boolean          | Should instrumentation collect payloads of operations                                                                                                                                                                 | `true`                      |

## Advanced Settings

#### Exporting Format

Data collected by the SDK is sent to Aspecto's collector in **protobuf** format by default. \
An alternative is to send the data in JSON format, which uses less CPU for encoding the message but is more verbose - causing more data to be sent over the network. \
To export data over JSON instead of protobuf, set the environment variable  `ASPECTO_TRACE_EXPORT_JSON` to any value.

#### Disabling Aspecto

Set the environment variable `DISABLE_ASPECTO` to any value, to disable Aspecto.\
Affect lambda and GCF wrappers as well.\
Useful when running unit tests, or as a simple kill switch.

####  **Live Flow**

Live flow allows you to capture flows from all the microservices that you're running locally (both on the host env and docker) with`local`mode enabled. To activate live flow mode use `local` option like so:

```javascript
require('@aspecto/opentelemetry')({ local: true });
```

 Once the process starts it will output the following link:

```
=====================================================================================================================================
|                                                                                                                                   |
| 🕵️‍♀️See the live tracing stream at https://app.aspecto.io/app/live-flows/sessions?instanceId=14243e72-14dc-4255-87af-ef846b247578   |
|                                                                                                                                   |
=====================================================================================================================================
```

Click on the link to open the Live Flow, to see traces from all the microservices that are running on your environment that have local mode enabled. The link is valid for a limited period of time (a couple of days, but it may change in the future). \
If you don't see a trace from some microservice (or none of them), click the newly-generated link.

##
