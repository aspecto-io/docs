# NodeJS



{% hint style="info" %}
The Aspecto SDK is an OpenTelemetry-based product. &#x20;
{% endhint %}

Install our Node.js library using `npm`:

```bash
$ npm install @aspecto/opentelemetry
```

#### Auto Instrument

1. Use node `-r` (or `--require`) [cli option](https://nodejs.org/api/cli.html#-r---require-module) to preload Aspecto SDK before application start:

```
-r @aspecto/opentelemetry/auto-instrument
```

e.g. your `node` invocation should look like this:

```
// before
node ./app

// after
node -r @aspecto/opentelemetry/auto-instrument ./app
```

2\. Set `aspectoAuth` and other optional SDK options via [environment variables or `aspecto.json` file](customize-defaults/advanced.md).

For example:

```
ASPECTO_AUTH="*your-token-goes-here*" node -r @aspecto/opentelemetry/auto-instrument ./app
```

#### Initialise in Code

After obtaining your token [here](https://app.aspecto.io/app/integration/api-key), **before any other import,** add the following call at the top of your app entry point:

{% tabs %}
{% tab title="JavaScript" %}
```javascript
require('@aspecto/opentelemetry')({
    local:true,
    aspectoAuth: '*your-token-goes-here*'
});
// ... other imports ...
```
{% endtab %}

{% tab title="TypeScript" %}
```typescript
import init from '@aspecto/opentelemetry';
init({
  local: true,
  aspectoAuth: '*your-token-goes-here*'
});
// ... other imports ...
```
{% endtab %}
{% endtabs %}

### Send traffic to your service

So far, you have configured your application to send telemetry to Aspecto for any inbound or outbound requests. Next, you will need to make requests to your service in order to generate the telemetry that will be sent to Aspecto.

Interact with your application by making a few requests. Telemetry data should now appear in the Aspecto UI!

Continue to the next step to start [investigate your tracing data](../../../observability-debugging/untitled/).

{% hint style="info" %}
See the [advanced page](customize-defaults/advanced.md) for specific configurations
{% endhint %}
