# Aspecto Overview

## 👁 Visualize Complex Traces

The Aspecto diagram shows the relations between services, the order of activities in a trace, and provides a clear picture of the app's architecture. Aspecto also displays the entire route of any message sent through Kafka, RabbitMQ, SQS, and other sorts of message brokers.

![](../.gitbook/assets/screen-shot-2021-03-16-at-11.52.53.png)

## [📈](https://emojipedia.org/chart-increasing/) Optimize Performance

The timeline displays the order of activities and displays how much time each activity took, making it easy to identify bottlenecks.

![](../.gitbook/assets/screen-shot-2021-03-16-at-11.30.25.png)

## 🗺  Follow a Parameter's Journey

Any parameter that appears inside a trace can be easily found: where it started, which components passed it along, and where it ended. This journey is visually marked on the graph and can be inspected inside each component's payload. 

![](../.gitbook/assets/screen-shot-2021-03-24-at-14.30.44.png)

## 🔎  Troubleshoot Issues in Staging and Production

Aspecto collects all trace data including requests, queries, and different types of data, then correlates it to logs and metrics. \
\
Beyond traditional logs, Aspecto allows you to easily search payloads, DB queries, async messages, and more, without being dependent on developers inserting logs in the correct places.

![](../.gitbook/assets/screen-shot-2021-03-16-at-13.45.28.png)

## 👋  Help New Developers Onboard Easily

New developers that join the team can get familiar with the app by inspecting microservices interactions. Each trace can be viewed separately, or as a part of the entire local data architecture. This makes it much easier to grasp how each service works and interacts with other services in the app.

![](../.gitbook/assets/bitmap.png)
