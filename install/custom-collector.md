# Custom Collector

This page is for advanced users which are already using opentelemetry and need their own collector.

Aspecto's SDK is by default exporting collected trace data to our collector at [https://otelcol-fast.aspecto.io/v1/trace](https://otelcol-fast.aspecto.io/v1/trace). Users that need this data for other purposes \(such as exporting it to Jaeger,  process it etc\), can configure Aspecto to send data to a custom collector by using the `otCollectorEndpoint` option:

```javascript
require('@aspecto/opentelemetry')({
    aspectoAuth: '*your-token-goes-here*',
    otCollectorEndpoint: 'https://{your-domain}:{port}/v1/trace',
});
```

Then, in your custom collector, you can export the spans to Aspecto's collector by defining relevant exporter in `config.yaml`:

```javascript
exporters:
  otlphttp:
    endpoint: https://otelcol-fast.aspecto.io
    
service:
  pipelines:
    traces:
      receivers: [...]
      processors: [...]
      exporters: [otlphttp, ...]
```

