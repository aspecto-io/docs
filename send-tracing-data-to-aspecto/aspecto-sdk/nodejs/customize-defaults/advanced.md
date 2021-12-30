---
description: Advanced configuration
---

# Advanced

### Configuration Methods

You can pass a configuration **options object** when initializing Aspecto in code, like this for example:

```javascript
require('@aspecto/opentelemetry')({
    packageName: 'my-package',
    env: 'production',
    samplingRatio: 0.1
});
```

Or add configuration **file** named `aspecto.json` near your `package.json` with the same options as above:

```
{
    packageName: 'my-package',
    env: 'production',
    samplingRatio: 0.1
}
```

Or use various environment variables, for example:

```
export ASPECTO_PACKAGE_NAME=my-package
```

Values are evaluated in the following priority:

1. `options` object
2. environment variables
3. config file
4. default values

### Configuration Options

\
Available install configurations (all are optional):

| Option                                    | Environment Variable                                   | Type             | Default                     | Description                                                                                                                                                                                                                                                                   |
| ----------------------------------------- | ------------------------------------------------------ | ---------------- | --------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `disableAspecto`                          | `DISABLE_ASPECTO`                                      | boolean          | `false` (aspecto enabled)   | Disable aspecto                                                                                                                                                                                                                                                               |
| `env`                                     | `NODE_ENV`                                             | string           | `process.env.NODE_ENV`      | Set environment name manually                                                                                                                                                                                                                                                 |
| `aspectoAuth`                             | `ASPECTO_AUTH`                                         | UUID             |                             | Set Aspecto token from code instead of using `aspecto.json`                                                                                                                                                                                                                   |
| `packageName`                             | `ASPECTO_PACKAGE_NAME`                                 | string           | "name" in `package.json`    | Set packageName manually instead of reading it from `package.json`. For example: a service that runs in multiple "modes"                                                                                                                                                      |
| `packageVersion`                          | `ASPECTO_PACKAGE_VERSION`                              | string           | "version" in `package.json` | Set packageVersion manually instead of reading it from `package.json`                                                                                                                                                                                                         |
| `samplingRatio`                           | `ASPECTO_SAMPLING_RATIO`                               | number           | `1.0`                       | How many of the traces starting in this service should be sampled. Set to number in range \[0.0, 1.0] where `0.0` is no sampling, and `1.0` is sample all. Specific rules set via aspecto app takes precedence                                                                |
| `requireConfigForTraces`                  | `ASPECTO_REQUIRE_CONFIG_FOR_TRACES`                    | boolean          | `false`                     | When `true`, the SDK will not trace anything until remote sampling configuration arrives (few hundreds ms). Can be used to enforce sampling configuration is always applied, with the cost of losing traces generated during service startup.                                 |
| `logger`                                  | -                                                      | logger interface |                             | Logger to be used in this tracing library. Common use for debugging `logger: console`                                                                                                                                                                                         |
| `collectPayloads`                         | `ASPECTO_COLLECT_PAYLOADS`                             | boolean          | `true`                      | Should Aspecto SDK collect payloads of operations                                                                                                                                                                                                                             |
| `local`                                   | -                                                      | boolean          | `false`                     | When set to true, enable [live traces](https://www.npmjs.com/package/@aspecto/opentelemetry#live-traces)                                                                                                                                                                      |
| `exportBatchSize`                         | `ASPECTO_EXPORT_BATCH_SIZE`                            | number           | `100`                       | How many spans to batch in a single export to the collector                                                                                                                                                                                                                   |
| `exportBatchTimeoutMs`                    | `ASPECTO_EXPORT_BATCH_TIMEOUT_MS`                      | number           | `1000` (1s)                 | Maximum time in ms for batching spans before sending to collector                                                                                                                                                                                                             |
| `writeSystemLogs`                         | -                                                      | boolean          | `false`                     | If `true`, emit all log messages from Opentelemetry SDK to supplied logger if present, or to console if missing                                                                                                                                                               |
| `customZipkinEndpoint`                    | -                                                      | URL              |                             | Send all traces to additional Zipkin server for debug                                                                                                                                                                                                                         |
| `sqsExtractContextPropagationFromPayload` | `ASPECTO_SQS_EXTRACT_CONTEXT_PROPAGATION_FROM_PAYLOAD` | boolean          | `true`                      | For aws-sdk instrumentation. Should be true when the service receiveMessages from SQS which is subscribed to SNS and subscription configured with "Raw message delivery": Disabled. Setting to `false` is a bit more performant as it turns off JSON parse on message payload |
| `extractB3Context`                        | `ASPECTO_EXTRACT_B3_CONTEXT`                           | boolean          | `false`                     | Set to `true` when the service receives requests from another instrumented component that propagate context via B3 protocol multi or single header. For example: Envoy Proxy, Ambassador and Istio                                                                            |
| `injectB3ContextSingleHeader`             | `ASPECTO_INJECT_B3_CONTEXT_SINGLE_HEADER`              | boolean          | `false`                     | Set to `true` when the service send traffic to another instrumented component that propagate context via B3 **single header** protocol                                                                                                                                        |
| `injectB3ContextMultiHeader`              | `ASPECTO_INJECT_B3_CONTEXT_MULTI_HEADER`               | boolean          | `false`                     | Set to `true` when the service send traffic to another instrumented component that propagate context via B3 **multi header** protocol. for example: Envoy Proxy, Istio                                                                                                        |

## Advanced Settings

#### Exporting Format

Data collected by the SDK is sent to Aspecto's collector in **protobuf** format by default. \
An alternative is to send the data in JSON format, which uses less CPU for encoding the message but is more verbose - causing more data to be sent over the network. \
To export data over JSON instead of protobuf, set the environment variable  `ASPECTO_TRACE_EXPORT_JSON` to any value.

#### Disabling Aspecto

Set the environment variable `DISABLE_ASPECTO` to any value, to disable Aspecto.\
Affect lambda and GCF wrappers as well.\
Useful when running unit tests, or as a simple kill switch.

#### &#x20;**Live Trace**

Live trace allows you to capture traces from all the microservices that you're running locally (both on the host env and docker) with`local`mode enabled. To activate live trace mode use `local` option like so:

```javascript
require('@aspecto/opentelemetry')({ local: true });
```

&#x20;Once the process starts it will output the following link:

```
=====================================================================================================================================
|                                                                                                                                   |
| 🕵️‍♀️See the live tracing stream at https://app.aspecto.io/app/live-traces/sessions?instanceId=14243e72-14dc-4255-87af-ef846b247578   |
|                                                                                                                                   |
=====================================================================================================================================
```

Click on the link to open the Live Trace, to see traces from all the microservices that are running on your environment that have local mode enabled. The link is valid for a limited period of time (a couple of days, but it may change in the future). \
If you don't see a trace from some microservice (or none of them), click the newly-generated link.

##
