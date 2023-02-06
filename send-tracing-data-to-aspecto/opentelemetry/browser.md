# Browser

Collecting traces from the browser will allow you to track user interaction. Traces will include the frontend operation that lead to backend trace.\
\
Instrument your web app using the OpenTelemetry JS SDK and send traces to Aspecto.

### Install Dependencies

{% tabs %}
{% tab title="npm" %}
```bash
npm install @opentelemetry/api @opentelemetry/sdk-trace-web @opentelemetry/sdk-trace-base @opentelemetry/context-zone @opentelemetry/resources @opentelemetry/semantic-conventions @opentelemetry/exporter-trace-otlp-http
```


{% endtab %}

{% tab title="yarn" %}
```bash
yarn add @opentelemetry/api @opentelemetry/sdk-trace-web @opentelemetry/sdk-trace-base @opentelemetry/context-zone @opentelemetry/resources @opentelemetry/semantic-conventions @opentelemetry/exporter-trace-otlp-http @opentelemetry/instrumentation
```
{% endtab %}
{% endtabs %}

### Install Instrumentation Libraries

You can choose instrumentation libraries that are relevant to you, for example:

{% tabs %}
{% tab title="npm" %}
```bash
npm install @opentelemetry/instrumentation-document-load @opentelemetry/instrumentation-xml-http-request @opentelemetry/instrumentation-user-interaction
```
{% endtab %}

{% tab title="yarn" %}
```bash
yarn add @opentelemetry/instrumentation-document-load @opentelemetry/instrumentation-xml-http-request @opentelemetry/instrumentation-user-interaction
```
{% endtab %}
{% endtabs %}

Or install **all** instrumentation libraries with the "@opentelemetry/auto-instrumentations-web" package:

{% tabs %}
{% tab title="npm" %}
```bash
npm install @opentelemetry/auto-instrumentations-web
```
{% endtab %}

{% tab title="yarn" %}
```bash
yarn add @opentelemetry/auto-instrumentations-web
```
{% endtab %}
{% endtabs %}

### Register OpenTelemetry SDK

Add this code to your application and invoke `registerOpenTelemetry` as early as possible to instrument your code.

<pre class="language-javascript"><code class="lang-javascript"><strong>import { WebTracerProvider } from '@opentelemetry/sdk-trace-web';
</strong>import { BatchSpanProcessor } from '@opentelemetry/sdk-trace-base';
import { ZoneContextManager } from '@opentelemetry/context-zone';
import { Resource, detectResources, browserDetector } from '@opentelemetry/resources';
import { SemanticResourceAttributes } from '@opentelemetry/semantic-conventions';
import { OTLPTraceExporter } from '@opentelemetry/exporter-trace-otlp-http';

import { registerInstrumentations } from '@opentelemetry/instrumentation';
import { DocumentLoadInstrumentation } from '@opentelemetry/instrumentation-document-load';
import { XMLHttpRequestInstrumentation } from '@opentelemetry/instrumentation-xml-http-request';
import { UserInteractionInstrumentation } from '@opentelemetry/instrumentation-user-interaction';

const aspectoToken = '--YOUR ASPECTO TOKEN--';
const serviceName = '--YOUR SERVICE NAME--';

// in order to propagate the context to your instrumented services, add their urls by matching a regex
const propagateTraceHeaderCorsUrls = [/my\.company\.domain/];

// urls that match any regex in ignoreUrls will not be traced and the context will not be propagate.
// it can be used to ignore uninteresting spans from the traces, avoid CORS issues, and ignore third party API calls
const ignoreUrls = [/third\.parties.\urls/];

export const registerOpenTelemetry = async () => {
    const autoResource = await detectResources({
        detectors: [browserDetector],
    });

    const resource = new Resource({
        [SemanticResourceAttributes.SERVICE_NAME]: serviceName,
    }).merge(autoResource);

    const provider = new WebTracerProvider({
        resource,
    });

    const aspectoExporter = new OTLPTraceExporter({
        url: 'https://otelcol.aspecto.io/v1/traces',
        headers: {
            Authorization: aspectoToken,
        },
    });
    provider.addSpanProcessor(new BatchSpanProcessor(aspectoExporter));

    provider.register({
        contextManager: new ZoneContextManager(),
    });

    registerInstrumentations({
        instrumentations: [
            new DocumentLoadInstrumentation(),
            new XMLHttpRequestInstrumentation({
                propagateTraceHeaderCorsUrls,
                ignoreUrls,
            }),
            new UserInteractionInstrumentation(),
            // add any other instrumentations you want to use
        ],
    });
};
</code></pre>

You'll need to set up the following in order to export traces to Aspecto:

* `aspectoToken` - You can get your token [here](https://app.aspecto.io/86092cc0/integration/tokens).
* `serviceName` - The name of your web application. You will be able to filter and search for it in Aspecto.
* `propagateTraceHeaderCorsUrls` - In order to propagate the context to your instrumented services, add their urls by matching a regex.
* `ignoreUrls` - Urls that match any regex in `ignoreUrls` will not be traced and the context will not be propagate. It can be used to ignore uninteresting spans from the traces, avoid CORS issues, and ignore third party API calls.
