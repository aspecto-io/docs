# NodeJS

Send traces to aspecto directly from your code using [`exporter-collector`](https://www.npmjs.com/package/@opentelemetry/exporter-collector) . Here is an example Node.js TypeScript snippet:

```typescript
import { NodeTracerProvider } from '@opentelemetry/sdk-trace-node';
import { Resource } from '@opentelemetry/resources';
import { SemanticResourceAttributes } from '@opentelemetry/semantic-conventions';
import { SimpleSpanProcessor } from '@opentelemetry/sdk-trace-base';
import { CollectorTraceExporter } from '@opentelemetry/exporter-collector';
import { registerInstrumentations } from '@opentelemetry/instrumentation';

const provider = new NodeTracerProvider({
    resource: new Resource({
        [SemanticResourceAttributes.SERVICE_NAME]: 'my-service-name' // service name is required
    }),
});

provider.register();
provider.addSpanProcessor(
    new SimpleSpanProcessor(
        new CollectorTraceExporter({
            url: 'https://otelcol.aspecto.io/v1/trace',
            headers: {
                // Aspecto API-Key is required
                Authorization: process.env.ASPECTO_API_KEY
            }
        })
    )
);

registerInstrumentations({
  instrumentations: [
    // add auto instrumentations here for packages your app uses
  ],
});
```
