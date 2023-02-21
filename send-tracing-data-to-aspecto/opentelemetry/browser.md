# Browser

Collecting traces from the browser will allow you to track user interaction. Traces will include the frontend operation that lead to backend trace.\
\
Instrument your web app using the OpenTelemetry JS SDK and send traces to Aspecto.

## Step 1 - Install Depenendecies

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

## Step 2 - Install Instrumentation Libraries

You can choose to

### Either

&#x20;Install **all instrumentation libraries that are relevant** to you, for example:

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

This option is friendlier for your bundle size as it only pulls in packages that are actually used.

A list of instrumentations and usage instructions is available at [OpenTelemetry Registry](https://opentelemetry.io/ecosystem/registry/?s=Browser\&component=instrumentation\&language=js)&#x20;

### Or

Install **all** instrumentation libraries with the "@opentelemetry/auto-instrumentations-web" package:

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

## Step 3 - Add OpenTelemetry SDK Setup Code

Add this code to your application in a file named `opentelemetry.ts`

{% hint style="danger" %}
This code contains parts you need to fill in yourself. `ignoreUrls` should include all your third parties domains to avoid CORS errors.
{% endhint %}

<pre class="language-javascript"><code class="lang-javascript"><strong>import { WebTracerProvider } from '@opentelemetry/sdk-trace-web';
</strong>import { BatchSpanProcessor } from '@opentelemetry/sdk-trace-base';
import { ZoneContextManager } from '@opentelemetry/context-zone';
import { Resource, detectResources, browserDetector } from '@opentelemetry/resources';
import { SemanticResourceAttributes } from '@opentelemetry/semantic-conventions';
import { OTLPTraceExporter } from '@opentelemetry/exporter-trace-otlp-http';

import { registerInstrumentations } from '@opentelemetry/instrumentation';
// import the libraries you are using from Step 2
import { DocumentLoadInstrumentation } from '@opentelemetry/instrumentation-document-load';
import { XMLHttpRequestInstrumentation } from '@opentelemetry/instrumentation-xml-http-request';
import { UserInteractionInstrumentation } from '@opentelemetry/instrumentation-user-interaction';

const aspectoToken = '--YOUR ASPECTO TOKEN--';
const serviceName = '--YOUR SERVICE NAME--';

// in order to propagate the context to your instrumented services, add their urls by matching a regex
const propagateTraceHeaderCorsUrls = [/my\.company\.domain/];

// urls that match any regex in ignoreUrls will not be traced and the context will not be propagate.
// it can be used to execlude uninteresting spans from the traces, avoid CORS issues, and ignore third party API calls
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

## Step 4 - Configure the Installation

Fill in the following i**n the code above** in order to export traces to Aspecto:

1\) `aspectoToken` - You can get your token [here](https://app.aspecto.io/86092cc0/integration/tokens).

2\) `serviceName` - The name of your web application. You will be able to filter and search for it in Aspecto trace viewer and use it for sampling rules and alerts.

3\) `propagateTraceHeaderCorsUrls` - In order to propagate the context to your instrumented services, add their urls by matching a regex.

This option is where you control which API calls in your frontend (browser) connect with traces in your backend ([nodejs](nodejs/) \ [java](java.md) \ [python](python.md) \ [ruby](ruby.md) \ .[NET](.net.md) \ [GO](go.md)).

4\) `ignoreUrls` - Urls that match any regex in `ignoreUrls` will not be traced and the context will not be propagated. It can be used to exclude uninteresting spans from the traces, avoid CORS issues, and ignore third-party API calls.

## Step 5 - Register OpenTelemetry in Your Code

Invoke the `registerOpenTelemetry` function in **your main `index.ts`** file as early as possible.

An example **React** startup might look similar to this:

```typescript
import { registerOpenTelemetry } from './opentelemetry';
import React from 'react';
import { render } from 'react-dom';
import App from './App';

registerOpenTelemetry();

const rootElement = document.getElementById('react-app');
render(<App />, rootElement);
```
