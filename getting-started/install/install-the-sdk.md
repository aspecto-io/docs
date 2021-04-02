# Install the SDK

Install our Node.js library using `npm`:

```bash
$ npm install @aspecto/opentelemetry
```

### Usage

Obtain your token [here](https://app.aspecto.io/app/integration/api-key).

Add this call at the top of your app entry point, **before any other import**:

```javascript
require('@aspecto/opentelemetry')({
    local:true,
    aspectoAuth: '*your-token-goes-here*'
});
// ... other imports ...
```

Once the process starts it will output the following link:

```text
=====================================================================================================================================
|                                                                                                                                   |
| 🕵️‍♀️See the live tracing stream at https://app.aspecto.io/app/live-flows/sessions?instanceId=14243e72-14dc-4255-87af-ef846b247578   |
|                                                                                                                                   |
=====================================================================================================================================
```

Click on the link to open the Live Flow to see traces from all the microservices that are running on your environment that have local mode enabled. The link is valid for a limited period of time.

{% hint style="info" %}
See the [advanced page](advanced.md) for specific configurations
{% endhint %}

