---
description: Send Traces to Aspecto directly from your code
---

# Using OpenTelemetry SDK

You need to use the [`exporter-collector`](https://www.npmjs.com/package/@opentelemetry/exporter-collector) , here's an example Node.js TypeScript snippet:

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



