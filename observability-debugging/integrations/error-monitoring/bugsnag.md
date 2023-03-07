---
description: Describe how to integrate Aspecto with Bugsnag
---

# Bugsnag

Bugsang allows you to collect application errors from your entire stack and pinpoint the issue you are trying to understand and solve. In most cases, especially in distributed applications, a single error lacks context as the error is the effect. But still, you need to have the whole picture to understand the cause of the error. Distributed traces will allow you to understand the cause and effect throughout services. Integrating your errors with traces will give you complete observability into your system.

When investing an error, you will be able to understand what other services were involved in the process.

It is a straightforward integration between Aspecto and Bugsnag. When reporting an error to Bugsang the OpenTelemetry trace id should be added as metadata so that it will be correlated to Aspecto.

Depending on the programming language, here is the first step, adding the trace id to any reported error:

{% tabs %}
{% tab title="JS" %}
```javascript
import { trace, context } from '@opentelemetry/api';
var Bugsnag = require('@bugsnag/js')
Bugsnag.start({
    apiKey: 'token goes here', //insert Bugsnag's api key
    onError: function (event: any) {
        const activeSpan = trace.getSpanContext(context.active());
        event.addMetadata('Aspecto', { traceId: activeSpan?.traceId }) // Adding traceId to any error
    }
})
```
{% endtab %}

{% tab title="Java" %}
```java
import io.opentelemetry.api.trace.*;

private final Tracer tracer = OpenTelemetry.getGlobalTracer("my_library_name", "1.0.0");

public String getActiveTraceId() {
    Span currentSpan = tracer.getCurrentSpan();
    if (currentSpan != null) {
        return currentSpan.getSpanContext().getTraceId();
    }
    return null;
}



bugsnag.addCallback(new Callback() {
    @Override
    public void beforeNotify(Report report) {
        report.addToTab("Aspecto", "traceId", getActiveTraceId());

    }
});

```
{% endtab %}

{% tab title="Python" %}
```python
from opentelemetry import trace

def get_active_trace_id():
    current_span = trace.get_current_span()
    if current_span:
        return current_span.get_span_context().trace_id
    return None

def callback(event):

    event.add_tab("Aspecto", {"traceId": get_active_trace_id()})

# Call `callback` before every event
bugsnag.before_notify(callback)

```
{% endtab %}

{% tab title="GO" %}
```go
import (
    "go.opentelemetry.io/otel"
)

func getActiveTraceID() string {
    tracer := otel.Tracer("")
    span := tracer.CurrentSpan()
    if span == nil {
        return ""
    }
    return span.SpanContext().TraceID().String()
}

bugsnag.Notify(err, ctx,
    bugsnag.MetaData{
        "Aspecto": {
            "traceId": getActiveTraceID(),
        },
    })

```
{% endtab %}

{% tab title=".NET" %}
```java
using System.Diagnostics;

public string GetActiveTraceId() {
    Activity currentActivity = Activity.Current;
    if (currentActivity != null) {
        return currentActivity.TraceId.ToString();
    }
    return null;
}

bugsnag.BeforeNotify(report => {
  report.Event.Metadata.Add("Aspecto", new Dictionary<string, object> { { "traceId", GetActiveTraceId() } });
});

```
{% endtab %}

{% tab title="Ruby" %}
```ruby
require 'opentelemetry'

def get_active_trace_id
  current_span = OpenTelemetry::Trace.current_span
  if current_span
    return current_span.span_context.trace_id
  end
  return nil
end

Bugsnag.configure do |config|
  config.add_on_error(proc do |event|
    # Add customer information to every event
    event.add_metadata(:Aspecto, {
      traceId: get_active_trace_id(),
    })
 
  end)
end

# Your app code here

```
{% endtab %}
{% endtabs %}

In Bugsang you will have a tab called "Aspecto", you can grab that traceId from there and search for it in Aspecto.

![](<../../../.gitbook/assets/image (8).png>)

