---
description: How to send traces to Aspecto without using our SDK
---

# Use w/o Aspecto SDK

To enjoy Aspecto to the fullest, we encourage you to use our SDK to send traces.  
If you already have an existing OpenTelemetry setup and prefer to keep using it, there are a couple of ways to do it.

The minimum required for our collector to accept your traces is to have the `service.name` resource attribute on your spans and pass the [Aspecto API-Key](https://app.aspecto.io/app/integration/api-key), either through `aspecto.token` resource attribute, or through the `Authorization` HTTP header when making a request to our collector.

## Send Traces to Aspecto directly from your code

To do so, you'd need to use the [`exporter-collector`](https://www.npmjs.com/package/@opentelemetry/exporter-collector) , here's an example Node.js snippet:

```typescript
import { NodeTracerProvider } from '@opentelemetry/node';
import { Resource } from '@opentelemetry/resources';
import { SimpleSpanProcessor } from '@opentelemetry/tracing';
import { CollectorTraceExporter } from '@opentelemetry/exporter-collector';

const provider = new NodeTracerProvider({
    resource: new Resource({
        'service.name': 'my-service-name' // service.name is required
    }),
});

provider.register();
provider.addSpanProcessor(
    new SimpleSpanProcessor(
        new CollectorTraceExporter({
            url: 'https://otelcol-fast.aspecto.io/v1/trace',
            headers: {
                // Aspecto API-Key is required
                Authorization: process.env.ASPECTO_API_KEY
            }
        })
    )
);
```

## Export to Aspecto Collector from your own OpenTelemetry Collector

If you already have your own [OpenTelemetry Collector](https://github.com/open-telemetry/opentelemetry-collector) and want to export your traces to Aspecto, you'd need to add a new `otlphttp` exporter in your config.yml:

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

