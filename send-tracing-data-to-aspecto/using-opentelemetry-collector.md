---
description: Export to Aspecto Collector from your own OpenTelemetry Collector
---

# Using OpenTelemetry Collector

If you already have your own [OpenTelemetry Collector](https://github.com/open-telemetry/opentelemetry-collector) and want to export your traces to Aspecto, you'd need to add a new `otlphttp` exporter in your config.yml, with Aspecto API-Key as your Authorization header:

```yaml
exporters:
  otlphttp:
    endpoint: https://otelcol-fast.aspecto.io
    headers:
      Authorization: <aspecto-api-key>
    
service:
  pipelines:
    traces:
      receivers: [...]
      processors: [...]
      exporters: [otlphttp, ...]
```

