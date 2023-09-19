---
description: Describe how to integrate Aspecto with BugSnag
---

# BugSnag

**BugSnag Integration with Aspecto**

BugSnag enables you to collect application errors across your entire stack, helping you identify and address issues efficiently. In distributed applications, errors often lack context, representing only the effect rather than the cause. To gain a comprehensive understanding of the error's root cause, you need visibility into your entire system. Distributed traces provide this insight by tracing the cause-and-effect relationships across your services. By integrating your errors with traces, you achieve complete observability of your system.

**Investigating Errors**

When investigating an error, it's essential to understand which other services were involved in the process. Integrating BugSnag with Aspecto is a straightforward process. When reporting an error to BugSnag, you have the option to include the following parameters based on your specific needs:

1. **Trace ID (Required)**: To correlate your error with Aspecto, ensure you include the OpenTelemetry trace ID as metadata when reporting the error.
2. **Workspace ID (Optional)**: Optionally, you can include the Workspace ID, which can be found in the Aspecto platform. If not provided, the link will point to the default workspace.
3. **Sampled Flag (Optional)**: In situations where the trace during the error was not sampled, you can include the Sampled Flag in the error report. In BugSnag's user interface, a message will indicate that the trace won't be available in Aspecto due to sampling.

**Getting Started**

Depending on your programming language, the first step is to add the trace ID and any optional parameters to your reported error. This ensures that you have complete visibility into the error's context, and, if needed, the ability to link it to specific Aspecto workspaces.

By following these steps, you can effectively integrate BugSnag with Aspecto, enabling you to better understand and resolve errors in your distributed application environment.

{% tabs %}
{% tab title="JS" %}
```javascript
import { trace, context } from '@opentelemetry/api';
import Bugsnag from '@bugsnag/js'

Bugsnag.start({
    apiKey: 'api key goes here', //insert Bugsnag's api key
    onError: function (event: any) {
        const activeSpan = trace.getSpanContext(context.active());
        const sampled = activeSpan.traceFlags.sampled;
        event.addMetadata('Aspecto', { 
            trace_id: activeSpan?.traceId,
            workspace_id: 'optinal workspace id',// Optinal, will use the default workspace if not provided
            sampled: sampled // Optinal if provided when the trace was not sampled the bugsnag UI will display appropriate message
      });
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
        report.addToTab("Aspecto", "trace_id", getActiveTraceId(), "workspace_id", "optinal workspace id");

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

    event.add_tab("Aspecto", {
        "trace_id": get_active_trace_id(),
        "workspace_id": "optinal workspace id", # Optinal, will use the defult one if not provided
        })

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
            "trace_id": getActiveTraceID(),
            "workspace_id": "optinal workspace id", // Optinal, will use default if not provided
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
  report.Event.Metadata.Add("Aspecto", new Dictionary<string, object> { { "trace_id", GetActiveTraceId(),  "workspace_id", "optinal workspace id" } });
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
      trace_id: get_active_trace_id(),
      workspace_id: "optinal workspace id", # Optinal, will use default if not provided
    })
 
  end)
end

# Your app code here

```
{% endtab %}
{% endtabs %}

In BugSnag you will have a tab called "Aspecto", there you will have a link to the trace in Aspecto.



<figure><img src="../../../.gitbook/assets/Screenshot 2023-09-19 at 10.12.21.png" alt=""><figcaption></figcaption></figure>
