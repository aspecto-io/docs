---
description: How Aspecto shows you the flows within your applications
---

# How it works

## OpenTelemetry

Aspecto uses [OpenTelemetry ](https://opentelemetry.io/docs/concepts/what-is-opentelemetry/)to collect information about the flows in your applications.   
OpenTelemetry is based on **distributed tracing**, which is a method used to profile and monitor applications, especially those built using a microservices architecture.  
  
This information is forwarded to Aspecto. In Aspecto, it is **analyzed** and rendered into Live Flow and other **visualizations** that you can use to understand and debug the actions in your applications down to component levels. 

## Auto-instrumentation

When you use Aspecto, your application is **instrumented** with our Node.js SDK \(see [Install](install/)\).  
Aspecto supports a number of Node.js libraries.   
As they are imported into your application, Aspecto automatically includes additional telemetry instrumentation plugin modules for them.

These Node.js libraries are supported: 

* http 
* https
* express 
* mongoose
* ioredis
* aws-sdk 
* kafkajs 
* typeorm 
* sequelize
* neo4j-driver
* @elastic/elasticsearch

Some of the plugins were developed by Aspecto and available as open-source.  
Check them out here: [https://github.com/aspecto-io/opentelemetry-ext-js](https://github.com/aspecto-io/opentelemetry-ext-js)



