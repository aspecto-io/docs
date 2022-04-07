---
description: Aspecto's SDK for ruby.
---

# Ruby

## Aspecto::OpenTelemetry

This gem is a distribution of OpenTelemetry pre-configured to use all available instrumentations and export trace data to Aspecto.

### Installation

Install the gem using:

```
$ gem install aspecto-opentelemetry
```

Or, if you use [bundler](https://bundler.io), include aspecto-opentelemetry in your Gemfile.

### Usage

#### Rails Applications

Add this code to a new file `aspecto.rb` under `config/initializers/`:

```
# config/initializers/aspecto.rb
require 'aspecto/opentelemetry'

Aspecto::OpenTelemetry::configure do |c|
  c.service_name = '<YOUR_SERVICE_NAME>'
  c.aspecto_auth = '<YOUR_ASPECTO_TOKEN>'
  # c.sampling_ratio = 1.0 # [optional]: defaults to 1.0, use aspecto app to configure remotely
end
```

#### Ruby Applications

Add this code after your require other gems:

```
require 'aspecto/opentelemetry'

Aspecto::OpenTelemetry::configure do |c|
  c.service_name = '<YOUR_SERVICE_NAME>'
  c.aspecto_auth = '<YOUR_ASPECTO_TOKEN>'
  # c.env = '<CURRENT_ENVIRONMENT>' # [optional]: automatically detected for rails and sinatra
  # c.sampling_ratio = 1.0 # [optional]: defaults to 1.0, use aspecto app to configure remotely
end
```

#### Shutdown

Call this function when your application shuts down

```
Aspecto::OpenTelemetry::shutdown
```

### Configuration

You can set configuration via environment variables or via code. Values set in code takes precedence. The only required config options are [`aspecto_auth`](https://app.aspecto.io/app/integration/api-key) and `service_name`.

#### Configuration Options

| Option Name                          | Environment Variable                      | Type        | Default                                                                      | Description                                                                                                                                                                                                                                  |
| ------------------------------------ | ----------------------------------------- | ----------- | ---------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `aspecto_auth`                       | `ASPECTO_AUTH`                            | UUID string | -                                                                            | Aspecto's [API key](https://app.aspecto.io/app/integration/api-key) for authentication                                                                                                                                                       |
| `service_name`                       | `OTEL_SERVICE_NAME`                       | string      | -                                                                            | Name of the service which is sending telemetry                                                                                                                                                                                               |
| `env`                                | `ASPECTO_ENV`                             | string      | Extracted from Rails or Sinatra if used                                      | Deployment environment: `production` / `staging` / `development`, etc.                                                                                                                                                                       |
| `log_level`                          | `OTEL_LOG_LEVEL`                          | string      | `ERROR`                                                                      | `ERROR` / `WARN` / `INFO`, etc.                                                                                                                                                                                                              |
| `sampling_ratio`                     | `ASPECTO_SAMPLING_RATIO`                  | float       | 1.0                                                                          | How many of the traces starting in this service should be sampled. set to number in range \[0.0, 1.0] where 0.0 is no sampling, and 1.0 is sample all                                                                                        |
| `require_config_for_traces`          | `ASPECTO_REQUIRE_CONFIG_FOR_TRACES`       | boolean     | `false`                                                                      | When `true`, the SDK will not trace anything until remote sampling configuration arrives (few hundreds ms). Can be used to enforce sampling configuration is always applied, with the cost of losing traces generated during service startup |
| `otel_exporter_otlp_traces_endpoint` | `OTEL_EXPORTER_OTLP_TRACES_ENDPOINT`      | URL         | [`https://otelcol.aspecto.io/v1/trace`](https://otelcol.aspecto.io/v1/trace) | Url                                                                                                                                                                                                                                          |
| `extract_b3_context`                 | `ASPECTO_EXTRACT_B3_CONTEXT`              | boolean     | `false`                                                                      | Set to `true` when the service receives requests from another instrumented component that propagate context via B3 protocol multi or single header. For example: Envoy Proxy, Ambassador and Istio                                           |
| `inject_b3_context_single_header`    | `ASPECTO_INJECT_B3_CONTEXT_SINGLE_HEADER` | boolean     | `false`                                                                      | Set to `true` when the service send traffic to another instrumented component that propagate context via B3 **single header** protocol                                                                                                       |
| `inject_b3_context_multi_header`     | `ASPECTO_INJECT_B3_CONTEXT_MULTI_HEADER`  | boolean     | `false`                                                                      | Set to `true` when the service send traffic to another instrumented component that propagate context via B3 **multi header** protocol. For example: Envoy Proxy, Istio                                                                       |

