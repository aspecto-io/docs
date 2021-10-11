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

{% hint style="info" %}
The minimum required for our collector to accept your traces is to have the `service.name` resource attribute on your spans and pass the [Aspecto API-Key](https://app.aspecto.io/app/integration/api-key), either through `aspecto.token` resource attribute, or through the `Authorization` HTTP header when making a request to our collector.
{% endhint %}
