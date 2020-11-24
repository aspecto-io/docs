# How it works

## OpenTelemetry

* Aspecto is using opentelemetry - [https://opentelemetry.io/docs/concepts/what-is-opentelemetry/](https://opentelemetry.io/docs/concepts/what-is-opentelemetry/)
* using distributed tracing - [https://opentracing.io/docs/overview/what-is-tracing](https://opentracing.io/docs/overview/what-is-tracing)
* what is auto instrumentation \(install the package, automatically get instrumentation for the packages you use\)
* write supported libs we instrument

  These Node.js libraries have been instrumented for Aspecto, to provide data flow information:

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
  * link to our contribution repo - [https://github.com/aspecto-io/opentelemetry-ext-js](https://github.com/aspecto-io/opentelemetry-ext-js)

  

