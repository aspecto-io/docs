# Java

Auto instrument your Java application using [opentelemetry-java-instrumentation](https://github.com/open-telemetry/opentelemetry-java-instrumentation) and send traces to Aspecto's collector. 

### Installation

Download the latest Java agent 'JAR' from [the official repo in github](https://github.com/open-telemetry/opentelemetry-java-instrumentation/releases) and copy the `opentelemetry-javaagent-all.jar` file to your project.

### Run Your App With Auto Instrumentation

You'll need to set up the following environment variables to export traces to Aspecto:

* `OTEL_SERVICE_NAME` - set to the name of the service
* `OTEL_EXPORTER_OTLP_HEADERS` - set to `Authorization=${YOUR_ASPECTO_TOKEN}` . You can get your token [here](https://app.aspecto.io/app/integration/api-key).
* `OTEL_EXPORTER_OTLP_TRACES_ENDPOINT` - set to `https://otelcol.aspecto.io:4317`

Once finished, load the agent to your application using the `-javaagent` option:

```
java -javaagent:/path/to/the/downloaded/opentelemetry-javaagent-all.jar -jar your-app.jar
```

