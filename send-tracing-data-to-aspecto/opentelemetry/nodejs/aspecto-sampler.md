---
description: >-
  You can craft your own custom installation of OpenTelemetry NodeJS SDK and
  integrate Aspecto Sampler to benefit from advanced sampling capabilities.
---

# Aspecto Sampler

Aspecto sampler fetches remote sampling rules that you configure in Aspecto platform and applies them on traces that start in a given service.

### Install

npm

```bash
npm install @aspecto/opentelemetry-sampler
```

yarn

```bash
yarn add @aspecto/opentelemetry-sampler
```

### Usage

Create an instance of `AspectoSampler` and use it in your `TracerProvider`

{% tabs %}
{% tab title="Type Script" %}
```typescript
import { AspectoSampler } from '@aspecto/opentelemetry-sampler';

// create an instance of AspectoSampler
const aspectoSampler = new AspectoSampler({
    // aspectoAuth,
    // serviceName,
    // env,
    // requireConfigForTraces,
    // samplingRatio,
});

// Now, you can now use it wherever an OpenTelemetry sampler is acceptable

// NodeTracerProvider
import { NodeTracerProvider } from '@opentelemetry/sdk-trace-node';
const provider = new NodeTracerProvider({
    sampler: aspectoSampler,
});

// NodeSDK
import { NodeSDK } from '@opentelemetry/sdk-node';
const sdk = new NodeSDK({
    traceExporter: // add your trace exporter here
    sampler: aspectoSampler,
});
```
{% endtab %}

{% tab title="Java Script" %}
```javascript
const { AspectoSampler } = require('@aspecto/opentelemetry-sampler');

// create an instance of AspectoSampler
const aspectoSampler = new AspectoSampler({
    // aspectoAuth,
    // serviceName,
    // env,
    // requireConfigForTraces,
    // samplingRatio,
});

// Now, you can now use it wherever an OpenTelemetry sampler is acceptable

// NodeTracerProvider
const { NodeTracerProvider } = require('@opentelemetry/sdk-trace-node');
const provider = new NodeTracerProvider({
    sampler: aspectoSampler,
});

// BasicTracerProvider
const { BasicTracerProvider } = require('@opentelemetry/sdk-trace-base');
const provider = new BasicTracerProvider({
    sampler: aspectoSampler,
});
```
{% endtab %}
{% endtabs %}

### Configuration

You can configure the sampler via one or more of the following:

* `options` variable. for example: `new AspectoSampler({optionName: optionValue})`. This enables setting config options dynamically.
* Environment variables
* Add `aspecto.json` configuration file to the root directory, next to service's `package.json` file

Values are evaluated in the following priority:

1. `options` object
2. environment variables
3. config file
4. default values

Aspecto auth is required. Service name and env are recommended.

<table><thead><tr><th>Option Name</th><th width="185">Environment Variable</th><th width="104">Type</th><th width="94">Default</th><th>Description</th></tr></thead><tbody><tr><td><code>disableAspecto</code></td><td><code>DISABLE_ASPECTO</code></td><td>boolean</td><td><code>false</code></td><td>Disable the sampler and don't apply any sampling logic (sample nothing)</td></tr><tr><td><code>aspectoAuth</code></td><td><code>ASPECTO_AUTH</code></td><td>UUID</td><td>-</td><td><a href="https://app.aspecto.io/app/integration/api-key">Aspecto token</a> for authentication to read sampling configuration</td></tr><tr><td><code>env</code></td><td><code>NODE_ENV</code></td><td>string</td><td>-</td><td>Set environment for matching sampling rules</td></tr><tr><td><code>serviceName</code></td><td><code>OTEL_SERVICE_NAME</code></td><td>string</td><td>-</td><td>Set service name for matching sampling rules</td></tr><tr><td><code>samplingRatio</code></td><td><code>ASPECTO_SAMPLING_RATIO</code></td><td>number</td><td><code>1.0</code></td><td>This option is used as a fallback if no specific sampling rule is matched, and before remote sampling rule configuration is fetched. It controls how many of the traces starting in this service should be sampled. Set to number in range the [0.0, 1.0] where <code>0.0</code> is no sampling, and <code>1.0</code> is sample all.</td></tr><tr><td><code>requireConfigForTraces</code></td><td><code>ASPECTO_REQUIRE_CONFIG_FOR_TRACES</code></td><td>boolean</td><td><code>false</code></td><td>When <code>true</code>, the Sampler will not trace anything until the remote sampling configuration arrives (a few hundred ms). Can be used to enforce sampling configuration is always applied, with the cost of losing traces generated during service startup.</td></tr></tbody></table>
