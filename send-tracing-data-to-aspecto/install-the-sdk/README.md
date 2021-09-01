# Using Aspecto SDK

{% hint style="info" %}
The Aspecto SDK is an OpenTelemetry-based product.  
{% endhint %}

Install our Node.js library using `npm`:

```bash
$ npm install @aspecto/opentelemetry
```

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

Once the process starts, it will output the following link:

```text
=====================================================================================================================================
|                                                                                                                                   |
| 🕵️‍♀️See the live tracing stream at https://app.aspecto.io/app/live-flows/sessions?instanceId=14243e72-14dc-4255-87af-ef846b247578   |
|                                                                                                                                   |
=====================================================================================================================================
```

Click on the link to open 'Live Flows' and see traces from all the microservices that are running on your environment.   
The link is valid for a limited period of time.

{% hint style="info" %}
See the [advanced page](customize-defaults/advanced.md) for specific configurations
{% endhint %}

