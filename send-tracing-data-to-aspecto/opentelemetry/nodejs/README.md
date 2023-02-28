# NodeJS

Send traces to Aspecto directly from your code using [`exporter-trace-otlp-proto`](https://www.npmjs.com/package/@opentelemetry/exporter-trace-otlp-proto) .

## Step 1 - Install Dependencies

{% tabs %}
{% tab title="npm" %}
<pre class="language-bash"><code class="lang-bash"><strong>npm install @opentelemetry/api \
</strong><strong>    @opentelemetry/sdk-trace-base \
</strong><strong>    @opentelemetry/sdk-trace-node \
</strong><strong>    @opentelemetry/resources \
</strong><strong>    @opentelemetry/semantic-conventions \
</strong><strong>    @opentelemetry/exporter-trace-otlp-proto \
</strong><strong>    @opentelemetry/instrumentation
</strong></code></pre>
{% endtab %}

{% tab title="yarn" %}
```bash
yarn add @opentelemetry/api \
    @opentelemetry/sdk-trace-base \
    @opentelemetry/sdk-trace-node \
    @opentelemetry/resources \
    @opentelemetry/semantic-conventions \
    @opentelemetry/exporter-trace-otlp-proto \
    @opentelemetry/instrumentation
```
{% endtab %}
{% endtabs %}

## Step 2 - Install Instrumentation Libraries

Install **all** instrumentation libraries with the [@opentelemetry/auto-instrumentations-node](https://www.npmjs.com/package/@opentelemetry/auto-instrumentations-node) package along with other common instrumentations:

{% tabs %}
{% tab title="npm" %}
```bash
npm install @opentelemetry/auto-instrumentations-node \
    opentelemetry-instrumentation-elasticsearch \
    opentelemetry-instrumentation-express \
    opentelemetry-instrumentation-kafkajs \
    opentelemetry-instrumentation-neo4j \
    opentelemetry-instrumentation-sequelize \
    opentelemetry-instrumentation-typeorm
```
{% endtab %}

{% tab title="yarn" %}
```bash
yarn add @opentelemetry/auto-instrumentations-node \
    opentelemetry-instrumentation-elasticsearch \
    opentelemetry-instrumentation-express \
    opentelemetry-instrumentation-kafkajs \
    opentelemetry-instrumentation-neo4j \
    opentelemetry-instrumentation-sequelize \
    opentelemetry-instrumentation-typeorm
```
{% endtab %}
{% endtabs %}

## Step 3 - Add OpenTelemetry SDK Setup Code

Add this code to your application in a file named `opentelemetry.ts`

{% hint style="danger" %}
This code contains parts you need to fill in yourself in step 4
{% endhint %}

{% tabs %}
{% tab title="Type Script" %}
```typescript
import { NodeTracerProvider } from '@opentelemetry/sdk-trace-node';
import { Resource } from '@opentelemetry/resources';
import { SemanticResourceAttributes } from '@opentelemetry/semantic-conventions';
import { BatchSpanProcessor } from '@opentelemetry/sdk-trace-base';
import { OTLPTraceExporter } from '@opentelemetry/exporter-trace-otlp-proto';
import { registerInstrumentations } from '@opentelemetry/instrumentation';

// instrumentation libraries
import { getNodeAutoInstrumentations } from '@opentelemetry/auto-instrumentations-node';
import { ExpressInstrumentation } from 'opentelemetry-instrumentation-express';
import { ElasticsearchInstrumentation } from 'opentelemetry-instrumentation-elasticsearch';
import { KafkaJsInstrumentation } from 'opentelemetry-instrumentation-kafkajs';
import { Neo4jInstrumentation } from 'opentelemetry-instrumentation-neo4j';
import { SequelizeInstrumentation } from 'opentelemetry-instrumentation-sequelize';
import { TypeormInstrumentation } from 'opentelemetry-instrumentation-typeorm';

const aspectoToken = '--YOUR ASPECTO TOKEN--';
const serviceName = '--YOUR SERVICE NAME--';

const provider = new NodeTracerProvider({
    resource: new Resource({
        [SemanticResourceAttributes.SERVICE_NAME]: serviceName
    }),
});

provider.register();
provider.addSpanProcessor(
    new BatchSpanProcessor(
        new OTLPTraceExporter({
            url: 'https://otelcol.aspecto.io/v1/traces',
            headers: {
                // Aspecto API-Key is required
                Authorization: aspectoToken
            }
        })
    )
);

registerInstrumentations({
  instrumentations: [
    getNodeAutoInstrumentations({
        // some instrumentation is noisy and commonly not useful
        '@opentelemetry/instrumentation-fs': {
            enabled: false
        },
        '@opentelemetry/instrumentation-net': {
            enabled: false
        },
        '@opentelemetry/instrumentation-dns': {
            enabled: false
        },
        '@opentelemetry/instrumentation-express': {
            enabled: false
        },
    }),
    new ExpressInstrumentation(),
    new ElasticsearchInstrumentation(),
    new KafkaJsInstrumentation(),
    new Neo4jInstrumentation(),
    new SequelizeInstrumentation(),
    new TypeormInstrumentation(),
  ],
});
```
{% endtab %}

{% tab title="Java Script" %}
```javascript
const { NodeTracerProvider } = require('@opentelemetry/sdk-trace-node');
const { Resource } = require('@opentelemetry/resources');
const { SemanticResourceAttributes } = require('@opentelemetry/semantic-conventions');
const { BatchSpanProcessor } = require('@opentelemetry/sdk-trace-base');
const { OTLPTraceExporter } = require('@opentelemetry/exporter-trace-otlp-proto');
const { registerInstrumentations } = require('@opentelemetry/instrumentation');

// instrumentation libraries
const { getNodeAutoInstrumentations } = require('@opentelemetry/auto-instrumentations-node');
const { ExpressInstrumentation } = require('opentelemetry-instrumentation-express');
const { ElasticsearchInstrumentation } = require('opentelemetry-instrumentation-elasticsearch');
const { KafkaJsInstrumentation } = require('opentelemetry-instrumentation-kafkajs');
const { Neo4jInstrumentation } = require('opentelemetry-instrumentation-neo4j');
const { SequelizeInstrumentation } = require('opentelemetry-instrumentation-sequelize');
const { TypeormInstrumentation } = require('opentelemetry-instrumentation-typeorm');

const aspectoToken = '--YOUR ASPECTO TOKEN--';
const serviceName = '--YOUR SERVICE NAME--';

const provider = new NodeTracerProvider({
    resource: new Resource({
        [SemanticResourceAttributes.SERVICE_NAME]: serviceName
    }),
});

provider.register();
provider.addSpanProcessor(
    new BatchSpanProcessor(
        new OTLPTraceExporter({
            url: 'https://otelcol.aspecto.io/v1/traces',
            headers: {
                // Aspecto API-Key is required
                Authorization: aspectoToken
            }
        })
    )
);

registerInstrumentations({
  instrumentations: [
    getNodeAutoInstrumentations({
        // some instrumentation is noisy and commonly not useful
        '@opentelemetry/instrumentation-fs': {
            enabled: false
        },
        '@opentelemetry/instrumentation-net': {
            enabled: false
        },
        '@opentelemetry/instrumentation-dns': {
            enabled: false
        },
        '@opentelemetry/instrumentation-express': {
            enabled: false
        },
    }),
    new ExpressInstrumentation(),
    new ElasticsearchInstrumentation(),
    new KafkaJsInstrumentation(),
    new Neo4jInstrumentation(),
    new SequelizeInstrumentation(),
    new TypeormInstrumentation(),
  ],
});
```
{% endtab %}
{% endtabs %}

## Step 4 - Configure the Installation

Fill in the following i**n the code above** in order to export traces to Aspecto:

1\) `aspectoToken` - You can get your token [here](https://app.aspecto.io/86092cc0/integration/tokens).

2\) `serviceName` - The name of your web application. You will be able to filter and search for it in Aspecto trace viewer and use it for sampling rules and alerts.

## Step 5 - Register OpenTelemetry in Your Code

Invoke the code above in **your main `index.ts`** file by `import`ing / `require`ing it as early as possible.

{% tabs %}
{% tab title="Type Script" %}
```typescript
import './opentelemetry'; // you may need to change the import depending on where you saved `opentelemetry.ts`
```
{% endtab %}

{% tab title="Java Script" %}
```javascript
require('./opentelemetry'); // you may need to change the import depending on where you saved `opentelemetry.js`
```
{% endtab %}
{% endtabs %}

## Step 6 - View Traces

Start your service and invoke it. Go to [Aspecto app](https://app.aspecto.io/search) and view your nodejs traces.
