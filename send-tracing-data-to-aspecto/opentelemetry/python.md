# Python

Instrument your app using the `opentelemetry-instrumentation` package and export traces to Aspecto's opentelemetry collector.

### Install Opentelemetry Instrumentation Packages

```
pip install opentelemetry-instrumentation
pip install opentelemetry-distro 
pip install opentelemetry-exporter-otlp-proto-grpc
```

To create automatic traces from packages you use in your application (flask, requests, etc), you need to additionally install a relevant instrumentation package. For example, if you use `flask` in your application, you need to install the package `opentelemetry-instrumentation-flask` .

To print recommended instrumentation packages based on your active site-packages directory, run:

```
opentelemetry-bootstrap --action=requirements
```

To install these packages, run:

```
opentelemetry-bootstrap --action=install
```

You can also directly install a specific instrumentation package. A list of community instrumentation packages can be found in the opentelemetry-python-contrib repo [here](https://github.com/open-telemetry/opentelemetry-python-contrib/tree/main/instrumentation)

### Running Your App with Auto Instrumentation

You need to setup the following environment variables to export the traces to Aspecto:

* `OTEL_SERVICE_NAME` - set to the name of the service
* `OTEL_EXPORTER_OTLP_HEADERS` - set to `Authorization=${YOUR_ASPECTO_TOKEN}` . You can get your token [here](https://app.aspecto.io/app/integration/api-key).
* `OTEL_EXPORTER_OTLP_TRACES_ENDPOINT` - set to `https://otelcol.aspecto.io:4317`

Then, run your app with the `opentelemetry-instrument` command:

```
opentelemetry-instrument python app.py
```
