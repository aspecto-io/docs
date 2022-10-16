# Quick Start

Ready to gain visibility and a deeper understanding of your distributed system? Get started with Aspecto in just 3 easy steps:

1. [Sign up an Aspecto account ](https://app.aspecto.io/user/login)(it's free!)
2. Install the Aspecto SDK
3. Access and observe your tracing data from Aspecto UI

## Step 1: Sign Up for an Account&#x20;

[Use this link](https://app.aspecto.io/user/login) to sign up for an Aspecto account or navigate to the [Aspecto website](https://www.aspecto.io/) and select the **Get Started** button.&#x20;

Once you've created an account, you can play around with the Aspecto UI using our simulated playground system or you can continue to step 2 and install our SDK.&#x20;

{% hint style="info" %}
Not ready to commit? Use our [playground](https://app.aspecto.io/play/search) to get a glimpse of Aspecto in action.&#x20;
{% endhint %}

## Step 2: Install the Aspecto SDK&#x20;

There are several ways to send telemetry data to Aspecto. The instructions below show you how to add instrumentation to your applications using Aspecto SDK.\
The Aspecto SDK can be installed within the microservices in your local environment. Once you've installed the SDK and deploy, every time data is created on your microservice, it will appear within the Aspecto UI.

&#x20;Use the tabs below to see which option is recommended based on the language you will be using.

{% tabs %}
{% tab title="JavaScript" %}
Start by installing the Aspecto package using `npm`:&#x20;

```
$ npm install @aspecto/opentelemetry
```

\
At the beginning of each project, add the following code. Make sure to use your authentication token, which can be found [here](https://app.aspecto.io/app/integration/api-key).&#x20;

```javascript
require('@aspecto/opentelemetry')({
    aspectoAuth: '*your-token-goes-here*'
});
// ... other imports ...
```
{% endtab %}

{% tab title="TypeScript" %}
Start by installing the Aspecto package using `npm`:&#x20;

```
$ npm install @aspecto/opentelemetry
```

\
At the beginning of each project, add the following code. Make sure to use your authentication token, which can be found [here](https://app.aspecto.io/app/integration/api-key).&#x20;

```typescript
import init from '@aspecto/opentelemetry';
init({
  aspectoAuth: '*your-token-goes-here*'
});
// ... other imports ...
```
{% endtab %}

{% tab title="Rails" %}
**Installation**

Install the gem using:

```
$ gem install aspecto-opentelemetry
```

Or, if you use [bundler](https://bundler.io/), include aspecto-opentelemetry in your Gemfile.

****\
**Configuration in Code**

Add this code to a new file `aspecto.rb` under `config/initializers/`:

```
# config/initializers/aspecto.rb
require 'aspecto/opentelemetry'

Aspecto::OpenTelemetry::configure do |c|
  c.service_name = '<YOUR_SERVICE_NAME>'
  c.aspecto_auth = '<YOUR_ASPECTO_TOKEN>'
  # c.sampling_ratio = 1.0 # [optional]: defaults to 1.0, use aspecto app to configure remotely
end
```
{% endtab %}

{% tab title="Ruby" %}
**Installation**

Install the gem using:

```
$ gem install aspecto-opentelemetry
```

Or, if you use [bundler](https://bundler.io/), include aspecto-opentelemetry in your Gemfile.

****\
**Configuration in Code**

```
require 'aspecto/opentelemetry'

Aspecto::OpenTelemetry::configure do |c|
  c.service_name = '<YOUR_SERVICE_NAME>'
  c.aspecto_auth = '<YOUR_ASPECTO_TOKEN>'
  # c.env = '<CURRENT_ENVIRONMENT>' # [optional]: automatically detected for rails and sinatra
  # c.sampling_ratio = 1.0 # [optional]: defaults to 1.0, use aspecto app to configure remotely
end
```
{% endtab %}
{% endtabs %}

### Send traffic to your service

So far, you have configured your application to send telemetry to Aspecto for any inbound or outbound requests. Next, you will need to make requests to your service in order to generate the telemetry that will be sent to Aspecto.

Interact with your application by making a few requests. Telemetry data should now appear in the Aspecto UI!\
\
Continue to the next step to start access your tracing data.

## Step 3: Access Your Tracing Data&#x20;

Using the Trace Search tool, filter and sort through your trace data to find performance bottlenecks, errors, exceptions and application issues that have reached your production environment.

Once you've searched through and easily pinpointed where the problem lies, click on the trace to further investigate and understand why the problem exists.&#x20;

### Search

Select the **Trace Search** icon to view a list of every trace that has been collected. Each row represents a trace - actions that took place in your system.

<figure><img src="../.gitbook/assets/Trace Search (1).png" alt=""><figcaption></figcaption></figure>

### **Filter**

Use the filters in the search bar to locate a specific trace to view more information on the endpoint-to-endpoint transaction. You can filter your search using the open search field or by: environment, service name, HTTP method**,** etc. You can check all the common filters [here](https://docs.aspecto.io/v1/observability-debugging/untitled#filter).

![](<../.gitbook/assets/Trace Search filters (1).png>)

### **Sort**

Now that you've filtered your search and have narrowed down the list of traces, you can sort through the remaining traces by clicking on any column header. The traces will automatically sort from ascending to descending but you can change the order of the sort using the arrow that appears next to the column name.

![](<../.gitbook/assets/Trace search duration sort.png>)

### **Observe**

From the refined list, select the specific trace you've been searching for in order to view more information. \
Three main sections will appear: Diagram, Summary and Timeline.

![](<../.gitbook/assets/Trace viewer.png>)

{% hint style="info" %}
Learn more how to [Investigate Your Tracing Data](https://docs.aspecto.io/v1/observability-debugging/untitled).
{% endhint %}

## What's Next?

* Check out our [Advanced](https://docs.aspecto.io/v1/send-tracing-data-to-aspecto/aspecto-sdk/customize-defaults/advanced) guide to customize what data is sent to Aspecto and to enable customized settings according to your use case.&#x20;
* [Investigate your tracing data](https://docs.aspecto.io/v1/observability-debugging/untitled) and pinpoint areas that can be improved or fixed.&#x20;
* Want to deploy to production? Create [sampling rules](https://docs.aspecto.io/v1/settings/sampling-rules) to determine exactly what data should be sent to Aspecto in production.&#x20;
