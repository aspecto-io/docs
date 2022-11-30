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

| Option Name              | Environment Variable                | Type    | Default | Description                                                                                                                                                                                                                                                                                                     |
| ------------------------ | ----------------------------------- | ------- | ------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `disableAspecto`         | `DISABLE_ASPECTO`                   | boolean | `false` | Disable the sampler and don't apply any sampling logic (sample nothing)                                                                                                                                                                                                                                         |
| `aspectoAuth`            | `ASPECTO_AUTH`                      | UUID    | -       | [Aspecto token](https://app.aspecto.io/app/integration/api-key) for authentication to read sampling configuration                                                                                                                                                                                               |
| `env`                    | `NODE_ENV`                          | string  | -       | Set environment for matching sampling rules                                                                                                                                                                                                                                                                     |
| `serviceName`            | `OTEL_SERVICE_NAME`                 | string  | -       | Set service name for matching sampling rules                                                                                                                                                                                                                                                                    |
| `samplingRatio`          | `ASPECTO_SAMPLING_RATIO`            | number  | `1.0`   | This option is used as a fallback if no specific sampling rule is matched, and before remote sampling rule configuration is fetched. It controls how many of the traces starting in this service should be sampled. Set to number in range the \[0.0, 1.0] where `0.0` is no sampling, and `1.0` is sample all. |
| `requireConfigForTraces` | `ASPECTO_REQUIRE_CONFIG_FOR_TRACES` | boolean | `false` | When `true`, the Sampler will not trace anything until the remote sampling configuration arrives (a few hundred ms). Can be used to enforce sampling configuration is always applied, with the cost of losing traces generated during service startup.                                                          |
