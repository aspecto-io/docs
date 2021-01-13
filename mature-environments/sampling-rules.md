---
description: Control how different flows in you service are sampled using remote rules
---

# Sampling Rules

When initializing Aspecto SDK, you can pass the **samplingRatio** option to control the general sampling rate of our client. But sometimes you'd want more sampling control only over specific flows. That's where sampling rules come in handy.

## Creating Rules

Go to the [sampling rules page](https://app.aspecto.io/app/settings/sampling-rules) on Aspecto website, and click on the "New Rule" button.  
You'd be presented with the rule creation dialog:

![](../.gitbook/assets/image%20%289%29.png)

In the above example, we are creating a rule to sample only 10% of _health-check_ requests where the service name is "_wikipedia-service"_ and the environment starts with "_prod"_.  
  
Setting a rule matcher for Service Name and Environment is mandatory.  
You can then choose to create extra conditions for your rules.  
Available matchers are:

* HTTP Request Path \(i.e. _/api/users/5ffc067eb3fe9c9520611f34_\)
* HTTP Full URL \(i.e. _https://my.domain.com/api/users?page=6&size=10_\)
* HTTP Method \(i.e. GET, POST, etc...\)
* HTTP Host \(i.e. _my.domain.com_\)
* User Agent \(i.e. _PostmanRuntime/7.2\)_

## **Important Notes**

* **All** rule conditions must be fulfilled for a rule to apply! 
* String-based rules are **case insensitive** \(i.e. "HELLO" contains "he" will match\) 
* SDK clients are updated in real-time and changes are **effective immediately**, so be careful! 
* Rules are run by their priority where **1 is the top priority.**  Once a rule is matched the other rules won't be run on the targeted flow anymore. 
* If non of the rules match, the default sampling rate will be used \(_samplingRate_ option or 100% if none provided\).

