---
description: Aspecto's SDK for ruby.
---

# Ruby

## Aspecto::OpenTelemetry

Aspecto's SDK for ruby. This gem is a distribution of OpenTelemetry configured to use all available instrumentations and export trace data to Aspecto.

### Installation

Add this line to your application's Gemfile:

```ruby
gem 'aspecto-opentelemetry'
```

And then execute:

```
$ bundle install
```

Or install it yourself as:

```
$ gem install aspecto-opentelemetry
```

### Usage

#### Auto Instrumentation

Does not require code changes. Just modify you `Gemfile` to add `require` like this:

```
gem 'aspecto-opentelemetry', require: 'aspecto/auto_instrument'
```

When using auto-instrumentation, you need to pass relevant configuration options via environment variables.

#### Manual Instrumentation

Allow you to configure the SDK via code:

```rb
require 'aspecto/opentelemetry'

Aspecto::OpenTelemetry::configure
```

Add this code at the beginning of your application. For rails - add this code to a file `aspecto.rb` under `config/initializers/`.

### Configuration

You can set configuration it 2 ways: via environment variables or via code. The only required config options are `aspecto_auth` and `service_name`.

#### Configuration in Code

```rb
require 'aspecto/opentelemetry'

Aspecto::OpenTelemetry::configure do |c|
  c.service_name = 'MY_SERIVCE'
  c.aspecto_auth = 'YOUR_TOKEN_HERE'
  c.env = 'MY_COOL_ENV'
  c.sampling_ratio = 0.1
end
```

#### Configuration Options

| Option Name      | Environment Variable     | Type        | Default                                 | Description                                                                                                                                           |
| ---------------- | ------------------------ | ----------- | --------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------- |
| `aspecto_auth`   | `ASPECTO_AUTH`           | UUID string | -                                       | Aspecto's [API key for authentication](https://app.aspecto.io/app/integration/api-key)                                                                |
| `service_name`   | `OTEL_SERVICE_NAME`      | string      | -                                       | name of the service which is sending telemetry                                                                                                        |
| `env`            | `ASPECTO_ENV`            | string      | extracted from rails or sinatra if used | deployment environment: `production` / `staging` / `development`, etc.                                                                                |
| `log_level`      | `OTEL_LOG_LEVEL`         | string      | `ERROR`                                 | `ERROR` / `WARN` / `INFO`, etc.                                                                                                                       |
| `sampling_ratio` | `ASPECTO_SAMPLING_RATIO` | float       | 1.0                                     | How many of the traces starting in this service should be sampled. set to number in range \[0.0, 1.0] where 0.0 is no sampling, and 1.0 is sample all |

