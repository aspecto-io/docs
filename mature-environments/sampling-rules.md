---
description: Control how different flows in you service are sampled using remote rules
---

# Sampling Rules

When initializing Aspecto SDK, you can pass the **`samplingRatio`** option to control the general sampling rate of our client. But sometimes you'd want more sampling control only over specific flows. That's where sampling rules come in handy.

## Creating Rules

Go to the [sampling rules page](https://app.aspecto.io/app/settings/sampling-rules) on Aspecto website, and click on the "New Rule" button.  
You'd be presented with the rule creation dialog:

![](../.gitbook/assets/image%20%289%29.png)

In the above example, we are creating a rule to sample only 10% of _`health-check`_ requests where the service name is `wikipedia-service`and the environment starts with `prod`.  
  
Setting a rule matcher for Service Name and Environment is mandatory.  
You can then choose to create extra conditions for your rules.  
Possible matchers for the extra conditions are:

* HTTP Request Path \(i.e. `/api/users/5ffc067eb3fe9c9520611f34`\)
* HTTP Full URL \(i.e. `https://my.domain.com/api/users?page=6&size=10`\)
* HTTP Method \(i.e. `GET`, `POST`, etc.\)
* HTTP Host \(i.e. `my.domain.com`\)
* User Agent \(i.e. `PostmanRuntime/7.2` \) 
* Queue/Topic Name \(Queue name for SQS, Topic name for Kafka, etc.\)

## **Important Notes**

* **All** rule conditions must be fulfilled for a rule to apply. 
* Rules are matched only against the **entry point** of a flow, but affect the sampling of the entire flow. That means that if I set a rule where `HTTP Request Path equals "/user"` , and my flow starts with a request to an endpoint called `/account` , which internally calls the `/user` endpoint, the rule will not apply, even though the user endpoint was involved.  
* String-based rules are **case insensitive** \(i.e. `"HELLO" contains "he"` will match\) 
* SDK clients are updated in real-time and changes are **effective immediately**, so be careful! 
* Rules are run by their priority where **1 is the top priority.**  Once a rule is matched the other rules won't be run on the targeted flow anymore. 
* If non of the rules match, the default sampling rate will be used \(`samplingRate` option or 100% if none provided\). 
* Sampling rules are ignored when running in local mode.

