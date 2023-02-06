# NodeJS

Send traces to Aspecto directly from your code using [`exporter-trace-otlp-proto`](https://www.npmjs.com/package/@opentelemetry/exporter-trace-otlp-proto) . Here is an example Node.js TypeScript and JavaScript snippets:

{% tabs %}
{% tab title="Type Script" %}
```typescript
import { NodeTracerProvider } from '@opentelemetry/sdk-trace-node';
import { Resource } from '@opentelemetry/resources';
import { SemanticResourceAttributes } from '@opentelemetry/semantic-conventions';
import { SimpleSpanProcessor } from '@opentelemetry/sdk-trace-base';
import { OTLPTraceExporter } from '@opentelemetry/exporter-trace-otlp-proto';
import { registerInstrumentations } from '@opentelemetry/instrumentation';

const provider = new NodeTracerProvider({
    resource: new Resource({
        [SemanticResourceAttributes.SERVICE_NAME]: 'my-service-name' // service name is required
    }),
});

provider.register();
provider.addSpanProcessor(
    new SimpleSpanProcessor(
        new OTLPTraceExporter({
            url: 'https://otelcol.aspecto.io/v1/traces',
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
{% endtab %}

{% tab title="Java Script" %}
```javascript
const { NodeTracerProvider } = require('@opentelemetry/sdk-trace-node');
const { Resource } = require('@opentelemetry/resources');
const { SemanticResourceAttributes } = require('@opentelemetry/semantic-conventions');
const { SimpleSpanProcessor } = require('@opentelemetry/sdk-trace-base');
const { OTLPTraceExporter } = require('@opentelemetry/exporter-trace-otlp-proto');

const provider = new NodeTracerProvider({
    resource: new Resource({
        [SemanticResourceAttributes.SERVICE_NAME]: 'my-service-name' // service name is required
    }),
});

provider.register();
provider.addSpanProcessor(
    new SimpleSpanProcessor(
        new OTLPTraceExporter({
            url: 'https://otelcol.aspecto.io/v1/traces',
            headers: {
                // Aspecto API-Key is required
                Authorization: process.env.ASPECTO_API_KEY
            }
        })
    )
);

```
{% endtab %}
{% endtabs %}
