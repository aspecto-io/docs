---
description: Advanced configuration
---

# Advanced

### Configuration Methods

You can pass a configuration **options object** when initializing Aspecto in code, like this for example:

```javascript
require('@aspecto/opentelemetry')({
    serviceName: 'my-service',
    env: 'production',
    samplingRatio: 0.1
});
```

Or add configuration **file** named `aspecto.json` near your `package.json` with the same options as above:

```
{
    serviceName: 'my-service',
    env: 'production',
    samplingRatio: 0.1
}
```

Or use various environment variables, for example:

```
export OTEL_SERVICE_NAME=my-service
```

Values are evaluated in the following priority:

1. `options` object
2. environment variables
3. config file
4. default values

### Configuration Options

\
Available install configurations (all are optional):

<table data-header-hidden><thead><tr><th width="421">Option Name</th><th width="542">Environment Variable</th><th>Type</th><th width="202">Default</th><th width="203">Description</th></tr></thead><tbody><tr><td>Option</td><td>Environment Variable</td><td>Type</td><td>Default</td><td>Description</td></tr><tr><td><code>disableAspecto</code></td><td><code>DISABLE_ASPECTO</code></td><td>boolean</td><td><code>false</code> (aspecto enabled)</td><td>Disable aspecto</td></tr><tr><td><code>env</code></td><td><code>NODE_ENV</code></td><td>string</td><td><code>process.env.NODE_ENV</code></td><td>Set environment name manually</td></tr><tr><td><code>aspectoAuth</code></td><td><code>ASPECTO_AUTH</code></td><td>UUID</td><td></td><td>Set Aspecto token from code instead of using <code>aspecto.json</code></td></tr><tr><td><code>serviceName</code></td><td><code>OTEL_SERVICE_NAME</code></td><td>string</td><td>"name" in <code>package.json</code></td><td>Set serviceName manually instead of reading it from <code>package.json</code>. For example: a service that runs in multiple "modes"</td></tr><tr><td><code>serviceVersion</code></td><td><code>OTEL_SERVICE_VERSION</code></td><td>string</td><td>"version" in <code>package.json</code></td><td>Set serviceVersion manually instead of reading it from <code>package.json</code></td></tr><tr><td><code>samplingRatio</code></td><td><code>ASPECTO_SAMPLING_RATIO</code></td><td>number</td><td><code>1.0</code></td><td>How many of the traces starting in this service should be sampled. Set to number in range [0.0, 1.0] where <code>0.0</code> is no sampling, and <code>1.0</code> is sample all. Specific rules set via aspecto app takes precedence </td></tr><tr><td><code>requireConfigForTraces</code></td><td><code>ASPECTO_REQUIRE_CONFIG_FOR_TRACES</code></td><td>boolean</td><td><code>false</code></td><td>When <code>true</code>, the SDK will not trace anything until remote sampling configuration arrives (few hundreds ms). Can be used to enforce sampling configuration is always applied, with the cost of losing traces generated during service startup.</td></tr><tr><td><code>logger</code></td><td>-</td><td>logger interface</td><td></td><td>Logger to be used in this tracing library. Common use for debugging <code>logger: console</code></td></tr><tr><td><code>collectPayloads</code></td><td><code>ASPECTO_COLLECT_PAYLOADS</code></td><td>boolean</td><td><code>true</code></td><td>Should Aspecto SDK collect payloads of operations</td></tr><tr><td><code>otlpCollectorEndpoint</code></td><td><code>OTEL_EXPORTER_OTLP_TRACES_ENDPOINT</code></td><td>string</td><td><code>https://otelcol-fast.aspecto.io/v1/trace</code></td><td>Target URL to which the OTLP http exporter is going to send spans</td></tr><tr><td><code>exportBatchSize</code></td><td><code>ASPECTO_EXPORT_BATCH_SIZE</code></td><td>number</td><td><code>100</code></td><td>How many spans to batch in a single export to the collector</td></tr><tr><td><code>exportBatchTimeoutMs</code></td><td><code>ASPECTO_EXPORT_BATCH_TIMEOUT_MS</code></td><td>number</td><td><code>1000</code> (1s)</td><td>Maximum time in ms for batching spans before sending to collector</td></tr><tr><td><code>writeSystemLogs</code></td><td>-</td><td>boolean</td><td><code>false</code></td><td>If <code>true</code>, emit all log messages from Opentelemetry SDK to supplied logger if present, or to console if missing</td></tr><tr><td><code>customZipkinEndpoint</code></td><td>-</td><td>URL</td><td></td><td>Send all traces to additional Zipkin server for debug</td></tr><tr><td><code>sqsExtractContextPropagationFromPayload</code></td><td><code>ASPECTO_SQS_EXTRACT_CONTEXT_PROPAGATION_FROM_PAYLOAD</code></td><td>boolean</td><td><code>true</code></td><td>For aws-sdk instrumentation. Should be true when the service receiveMessages from SQS which is subscribed to SNS and subscription configured with "Raw message delivery": Disabled. Setting to <code>false</code> is a bit more performant as it turns off JSON parse on message payload</td></tr><tr><td><code>extractB3Context</code></td><td><code>ASPECTO_EXTRACT_B3_CONTEXT</code></td><td>boolean</td><td><code>false</code></td><td>Set to <code>true</code> when the service receives requests from another instrumented component that propagate context via B3 protocol multi or single header. For example: Envoy Proxy, Ambassador and Istio</td></tr><tr><td><code>injectB3ContextSingleHeader</code></td><td><code>ASPECTO_INJECT_B3_CONTEXT_SINGLE_HEADER</code></td><td>boolean</td><td><code>false</code></td><td>Set to <code>true</code> when the service send traffic to another instrumented component that propagate context via B3 <strong>single header</strong> protocol</td></tr><tr><td><code>injectB3ContextMultiHeader</code></td><td><code>ASPECTO_INJECT_B3_CONTEXT_MULTI_HEADER</code></td><td>boolean</td><td><code>false</code></td><td>Set to <code>true</code> when the service send traffic to another instrumented component that propagate context via B3 <strong>multi header</strong> protocol. For example: Envoy Proxy, Istio</td></tr></tbody></table>

## Advanced Settings

#### Exporting Format

Data collected by the SDK is sent to Aspecto's collector in **protobuf** format by default. \
An alternative is to send the data in JSON format, which uses less CPU for encoding the message but is more verbose - causing more data to be sent over the network. \
To export data over JSON instead of protobuf, set the environment variable  `ASPECTO_TRACE_EXPORT_JSON` to any value.

#### Disabling Aspecto

Set the environment variable `DISABLE_ASPECTO` to any value, to disable Aspecto.\
Affect lambda and GCF wrappers as well.\
Useful when running unit tests, or as a simple kill switch.
