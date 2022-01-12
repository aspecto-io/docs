---
description: Export traces to Aspecto from your Jaeger agent
---

# Using Jaeger Agent

If you already have a Jaeger agent in your environment, you can configure it to send traces to Aspecto.

You'd need to set the following CLI flags in your agent entry point

```
--reporter.grpc.host-port=collector.aspecto.io:14250
--reporter.grpc.tls.enabled=true
--agent.tags=aspecto.token=<aspecto-api-key>
```

If you use docker you can use the following code to add it&#x20;

```
docker run
--rm \
-p5775:5775/udp \
-p6831:6831/udp \
-p6832:6832/udp \
-p5778:5778/tcp \
jaegertracing/jaeger-agent:1.30 \
--reporter.grpc.host-port=collector.aspecto.io:14250 \
--reporter.grpc.tls.enabled=true \
--agent.tags=aspecto.token=<aspecto-api-key>
```

You can find your API key [here](https://app.aspecto.io/app/integration/api-key).
