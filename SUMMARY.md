# Table of contents

* [Documentation Home](README.md)

## 🎬 GETTING STARTED

* [Quick Start](getting-started/quick-start.md)
* [Aspecto Overview](getting-started/overview.md)
* [About Observability](getting-started/observability.md)

## 🚀 Send Tracing Data to Aspecto

* [How Do You Want to Send Your Data?](send-tracing-data-to-aspecto/send-tracing-data-to-aspecto.md)
* [Using Aspecto SDK](send-tracing-data-to-aspecto/aspecto-sdk/README.md)
  * [NodeJS](send-tracing-data-to-aspecto/aspecto-sdk/nodejs/README.md)
    * [NestJS](send-tracing-data-to-aspecto/aspecto-sdk/nodejs/nestjs.md)
    * [AWS Lambda](send-tracing-data-to-aspecto/aspecto-sdk/nodejs/lambda.md)
    * [Google Cloud Functions](send-tracing-data-to-aspecto/aspecto-sdk/nodejs/gcf.md)
    * [Customize Defaults](send-tracing-data-to-aspecto/aspecto-sdk/nodejs/customize-defaults/README.md)
      * [Advanced](send-tracing-data-to-aspecto/aspecto-sdk/nodejs/customize-defaults/advanced.md)
      * [Send Spans Manually](send-tracing-data-to-aspecto/aspecto-sdk/nodejs/customize-defaults/manual-spans.md)
      * [Add span attributes manually](send-tracing-data-to-aspecto/aspecto-sdk/nodejs/customize-defaults/add-span-attributes-manually.md)
      * [Send Logs](send-tracing-data-to-aspecto/aspecto-sdk/nodejs/customize-defaults/configure-logs.md)
      * [Correlate Logs with Traces](send-tracing-data-to-aspecto/aspecto-sdk/nodejs/customize-defaults/logs-correlation.md)
  * [Ruby](send-tracing-data-to-aspecto/aspecto-sdk/ruby.md)
* [Using OpenTelemetry SDK](send-tracing-data-to-aspecto/opentelemetry/README.md)
  * [NodeJS](send-tracing-data-to-aspecto/opentelemetry/nodejs.md)
  * [Java](send-tracing-data-to-aspecto/opentelemetry/java.md)
  * [Python](send-tracing-data-to-aspecto/opentelemetry/python.md)
  * [Ruby](send-tracing-data-to-aspecto/opentelemetry/ruby.md)
  * [.NET](send-tracing-data-to-aspecto/opentelemetry/.net.md)
  * [Go](send-tracing-data-to-aspecto/opentelemetry/go.md)
* [Using OpenTelemetry Collector](send-tracing-data-to-aspecto/using-opentelemetry-collector.md)
* [Using Jaeger Agent](send-tracing-data-to-aspecto/using-jaeger-agent.md)

## 👀 Observability & Debugging <a href="#observability-debugging" id="observability-debugging"></a>

* [Investigate Your Tracing Data](observability-debugging/untitled/README.md)
  * [Search for Performance Issues](observability-debugging/untitled/search-for-performance-issues.md)
  * [Search for Errors and Exceptions](observability-debugging/untitled/search-for-errors-and-exceptions.md)
* [Troubleshoot in Your Local Environment](observability-debugging/visualize-data-flows/README.md)
  * [Analyze Logs within a Trace](observability-debugging/visualize-data-flows/logging.md)
* [Integrations](observability-debugging/integrations/README.md)
  * [Logs](observability-debugging/integrations/logs/README.md)
    * [Coralogix](observability-debugging/integrations/logs/coralogix.md)
    * [Logz.io](observability-debugging/integrations/logs/logz.io.md)
  * [Error Monitoring](observability-debugging/integrations/error-monitoring/README.md)
    * [Rollbar](observability-debugging/integrations/error-monitoring/rollbar.md)
    * [Sentry](observability-debugging/integrations/error-monitoring/sentry.md)

## 🏆 Deploy to Production <a href="#settings" id="settings"></a>

* [Sampling Rules](settings/sampling-rules.md)
* [Data Privacy](settings/data-privacy.md)
* [Create Workspaces for Your Environments](settings/create-workspaces-for-your-environments.md)
