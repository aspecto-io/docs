---
description: how Aspecto shows you the flows within your applications
---

# How it works

## OpenTelemetry

Aspecto uses [OpenTelemetry ](https://opentelemetry.io/docs/concepts/what-is-opentelemetry/)to collect information about the the flows in your  applications. This information is forwarded to Aspecto. In Aspecto, it is analyzed and rendered into Live Flow and other visualizations that you can use to understand and debug the actions in your applications down to component levels. 

## Auto-instrumentation

When you use Aspecto, your application is instrumented with a telemetry module \(see [Install](install.md)\) for Node.js. Aspecto supports a number of Node.js libraries. As they are included in your application, Aspecto automatically includes additional telemetry instrumentation plugin modules for them.

These libraries are supported: 

* mongoose
* aws-sdk 
* kafka-node 
* kafkajs 
* express 
* typeorm 
* http 
* https
* ioredis
* sequelize

The plugin modules for these libraries are this open-source repository

[https://github.com/aspecto-io/opentelemetry-ext-js](https://github.com/aspecto-io/opentelemetry-ext-js)



