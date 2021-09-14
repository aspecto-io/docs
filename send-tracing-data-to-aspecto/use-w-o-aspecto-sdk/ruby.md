---
description: >-
  Instrument your Ruby on Rails app using opentelemetry, and export traces to
  aspecto's collector.
---

# Ruby

### Install Opentelemetry SDK and instrumentations

```text
gem install opentelemetry-sdk
gem install opentelemetry-exporter-otlp
gem install opentelemetry-instrumentation-all
```

### Initialize Opentelemetry

Add a rails initializer file `opentelemetry.rb`  under `config/initializers` with the content:

```text
require 'opentelemetry/sdk'
require 'opentelemetry/exporter/otlp'
require 'opentelemetry/instrumentation/all'
OpenTelemetry::SDK.configure do |c |
  c.service_name = '#{YOUR_SERVICE_NAME}'
  c.use_all() # enables all instrumentation!
  c.add_span_processor(
    OpenTelemetry::SDK::Trace::Export::BatchSpanProcessor.new(
      OpenTelemetry::Exporter::OTLP::Exporter.new(endpoint: 'https://otelcol.aspecto.io/v1/trace', headers: {
        'Authorization' => '#{YOUR_ASPECTO_TOKEN}'
      })
    )
  )
end
```

You need to substitute the values `#{YOUR_SERVICE_NAME}` with the name of your service, and `#{YOUR_ASPECTO_TOKEN}` with a token you can find [here](https://app.aspecto.io/app/integration/api-key)

