---
description: >-
  Control how different traces in your service are sampled using remote sampling
  rules
---

# Sampling Rules

## Trace Sampling

So what does it mean when we say sampling? \
It means to make a decision not to save all the traces that are created by your application, with the assumption that only some of them are enough to understand patterns in your services and gain better insights.&#x20;

As for the motivation: The main motivation for using it is either to save costs, or when you have some data that is clutter that you want out of your way.

When (and where) we can make the decision to sample? \
There are 2 approaches – **Head-Based Sampling & Tail-Based Sampling**.

## Head-Based Sampling

As the name suggests, head-based sampling means to make the decision to sample or not upfront, at the beginning of the trace.

This is the most common way of doing sampling as of today because of the simplicity, but since we don’t know everything in advance we’re forced to make arbitrary decisions (like a random percentage of all spans to sample) that may limit our ability to understand everything. This is done at the OTEL distro level.

### Create Head-Based Sample Rules

Go to the [sampling rules page](https://app.aspecto.io/app/settings/sampling-rules) on Aspecto website, and click on the **"New Rule"** button.\
You'd be presented with the rule creation dialog:

![Create new head sampling rule](<../.gitbook/assets/Create new Head Sampling Rule.png>)

In the above example, we are creating a rule to sample only 10% of _`health-check`_ requests where the service name is `wikipedia-service`and the environment starts with `prod`.\
\
Setting a rule matcher for Service Name and Environment is mandatory.\
You can then choose to create extra conditions for your rules.\
\
Possible matchers for the extra conditions are:

* HTTP Request Path (i.e. `/api/users/5ffc067eb3fe9c9520611f34`)
* HTTP Full URL (i.e. `https://my.domain.com/api/users?page=6&size=10`)
* HTTP Method (i.e. `GET`, `POST`, etc.)
* User Agent (i.e. `PostmanRuntime/7.2` )&#x20;
* Queue/Topic Name (Queue name for SQS, Topic name for Kafka, etc.)
* Span Name

{% hint style="info" %}
* Conditions work with an **and** operator, meaning all conditions must match the first span of a trace in order for it to be affected by the rule.
* Rules are matched only against the **entry point** of a flow but affect the sampling of the entire flow.\
  That means that if I set a rule where `HTTP Request Path equals "/user"` , and my flow starts with a request to an endpoint called `/account` , which internally calls the `/user` endpoint, the rule will not apply, even though the user endpoint was involved.&#x20;
{% endhint %}

## **Tail**-Based Sampling

Contrary to head-based sampling, here we make the decision at the end of the entire flow, when we already gathered the data. This type of sampling is done at the collector (the backend that receives all the spans) level.&#x20;

Tail Sampling looks into traces collected by the OpenTelemetry collector (the remaining traces after head sampling). With Tail Sampling you can create advanced rules to filter out traces based on any span property, including their results, attributes and duration. For example, tail-based sampling allow us to collect anomalous data, like long latency or a rare errors.\
However, in order to make a decision at the end of the workflow, the system has to buffer some information, which can increase storage overhead.

Set priority to each rule to make sure sampling is occurring in the right order and no important data is lost.\
To create a local OpenTelemetry collector contact us.

### Create Tail-Based Sample Rules

Go to the [sampling rules page](https://app.aspecto.io/app/settings/sampling-rules) on Aspecto website, and click on the **"New Rule"** button.\
You'd be presented with the rule creation dialog:

![Create new tail sampling rule](<../.gitbook/assets/Create new tail sampling rule - errors.png>)

In the above example, we are creating a rule to sample 100% of the traces that contains error where the service name is `wikipedia-service` and the environment equals `prod`.

![Create new tail sampling rule](<../.gitbook/assets/Create new tail sampling rule - duration.png>)

In the above example, we are creating a rule to sample 100% of the traces whose _`http.status_code`_ is `4xx` format and their duration > 15 seconds.\
\
You can then choose to create extra conditions for your rules.\
Possible matchers for tail-based sampling are (in addition to the head-based sampling matchers):

* Duration (i.e. traces with duration > 20 seconds)
* Errors (i.e. traces that contains/not contains errors)
* HTTP Status (i.e. `200`, `404`, `5xx`, etc.)
* Attribute key-value (i.e. traces that contains an attribute `userId` equals `12345`, etc.)

{% hint style="info" %}
* Conditions work with an **and** operator, meaning all conditions must match **at least one span of a trace** for it to be affected by the rule.
* Tail-based sampling rules are affected by the head-based sampling rules.\
  That means that if I set a tail-based rule to collect 100% of the traces where `Error is exist`, and I set also a head-based rule to collect 0% of the traces where `HTTP Request Path equals "/health-check".`When call to `"/health-check"`will fail and will contain error, the tail-based rule will not apply and the trace with the error will not be sampled because it already not  sampled by the head-based rule in the beginning of the trace.
* Every head sampling condition (﻿HTTP Request Path, HTTP Full URL, HTTP Method, User Agent, Queue/Topic Name) can be applied also as tail sampling.
{% endhint %}

## **Important Notes**

* String-based rules are **case insensitive** (i.e. `"HELLO" contains "he"` will match)
* SDK clients are updated in real-time and changes are **effective immediately**, so be careful!
* Rules are run by their priority where **1 is the top priority.** Once a rule matches a flow, no other rules will be applied to the same flow.
* If non of the rules match, the default sampling rate will be used (`samplingRate` option or 100% if none provided).
* Sampling rules are ignored when running in local mode.
* Sample Rate can be any valid floating-point number in the range \[0.0, 100.0].
* To create a local OpenTelemetry collector contact us.
