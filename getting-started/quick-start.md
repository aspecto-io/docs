# Quick Start

Ready to gain visibility and a deeper understanding of your distributed system? Get started with Aspecto in just 3 easy steps:

1. [Sign up an Aspecto account ](https://app.aspecto.io/user/login)(it's free!)
2. Install our SDK
3. Access and observe your tracing data&#x20;

## Step 1: Sign Up for an Account&#x20;

[Use this link](https://app.aspecto.io/user/login) to sign up for an Aspecto account or navigate to the [Aspecto website](https://www.aspecto.io) and select the **Get Started** button.&#x20;

Once you've created an account, you can play around with the Aspecto UI using our simulated playground system or you can continue to step 2 and install our SDK.&#x20;

{% hint style="info" %}
Not ready to commit? Use our [playground](https://app.aspecto.io/play/search) to get a glimpse of Aspecto in action.&#x20;
{% endhint %}

## Step 2: Install the Aspecto SDK&#x20;

The Aspecto SDK can be installed within the microservices in your local environment. Once you've installed the SDK, every time data is locally created on your microservice, it will appear within the Aspecto UI.

![](<../.gitbook/assets/image (15).png>)

Start by installing the Aspecto package using `npm`:&#x20;

```
$ npm install @aspecto/opentelemetry
```

At the beginning of each project, add the following code. Make sure to use your authentication token, which can be found [here](https://app.aspecto.io/app/integration/api-key).&#x20;

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

Once the process begins, a link will be outputted. Click on the link to open our **Live Traces** tool and view traces from every microservice that is running in your environment.&#x20;

```
=====================================================================================================================================
|                                                                                                                                   |
| 🕵️‍♀️See the live tracing stream at https://app.aspecto.io/app/live-traces/sessions?instanceId=14243e72-14dc-4255-87af-ef846b247578   |
|                                                                                                                                   |
=====================================================================================================================================
```

{% hint style="info" %}
The provided link is valid for a limited period of time.
{% endhint %}

## Step 3: Access Your Tracing Data&#x20;

The **Live Traces** tool within the Aspecto UI presents a list of all your live traces, making it easy to view every action that takes place in your system. It is a powerful way to proactively develop and debug your application, preventing issues from reaching production.

By clicking on any trace, view all microservice dependencies and how every component within your distributed system communicates or relies on one another.&#x20;

![](<../.gitbook/assets/image (13).png>)

Using the trace diagram, summary, and timeline, you can observe and understand how a code change will affect your entire system as well as pinpoint performance issues and other anomalies, like errors and exceptions.&#x20;

## Bonus: Taking Aspecto One Step Further&#x20;

👉  Check out our [Advanced](https://docs.aspecto.io/v1/send-tracing-data-to-aspecto/aspecto-sdk/customize-defaults/advanced) guide to customize what data is sent to Aspecto and to enable customized settings according to your use case.&#x20;

🔎 [Investigate your tracing data](https://docs.aspecto.io/v1/observability-debugging/untitled) and pinpoint areas that can be improved or fixed.&#x20;

🚀 Want to deploy to production? Create [sampling rules](https://docs.aspecto.io/v1/settings/sampling-rules) to determine exactly what data should be sent to Aspecto in production. \


