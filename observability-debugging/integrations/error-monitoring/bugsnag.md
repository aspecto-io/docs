---
description: Integrate Aspecto Distributed Tracing with BugSnag Error Monitoring
---

# BugSnag

BugSnag enables you to collect application errors across your entire stack, helping you identify and address issues efficiently. In distributed applications, errors often lack context, representing only the effect rather than the cause. To gain a comprehensive understanding of the error's root cause, you need visibility into your entire system. Distributed traces provide this insight by tracing the cause-and-effect relationships across your services. By integrating your errors with traces, you achieve complete observability of your system.

[Learn more about BugSnag Error Monitoring here](https://www.bugsnag.com/error-monitoring/)

**Investigating Errors**

By integrating Aspecto with BugSnag, your BugSnag errors will be linked to the relevant distributed trace within Aspecto, allowing you to see the full context of the error.

<figure><img src="../../../.gitbook/assets/Screenshot 2023-09-19 at 10.12.21.png" alt=""><figcaption></figcaption></figure>

You associate the error to the trace using the following metadata:

* `Aspecto.trace_id` (required): The ID of the OpenTelemetry trace that was current when the error occurred.
* `Aspecto.workspace_id` (optional): Optionally, you can include the Workspace ID, which can be found in the Aspecto platform. If not provided, the link will point to the default workspace. You can find your Workspace ID [here](https://app.aspecto.io/workspaces)
* `Aspecto.sampled` (optional, defaults to `true`): Set this to `true` or `false` depending on whether the trace containing the error was sampled. If it is `false`, the BugSnag dashboard will indicate that the trace won't be available in Aspecto.

**Code snippets**

To implement this integration, use a callback within your BugSnag setup code. See the snippets below for popular languages. If you need help integrating Aspecto for an unlisted language, please [contact us](mailto:support@aspecto.io).

{% tabs %}
{% tab title="JS" %}
```javascript
import { trace, context } from '@opentelemetry/api';
import Bugsnag from '@bugsnag/js'

Bugsnag.start({
    apiKey: 'api key goes here', //insert Bugsnag's api key
    onError: function (event: any) {
        const spanContext = trace.getSpanContext(context.active());
        const traceId = spanContext.traceId
        const sampled = spanContext.traceFlags === TraceFlags.SAMPLED;
        event.addMetadata('Aspecto', { 
            trace_id: traceId,
            workspace_id: 'optional workspace id',// Optional,
            sampled: sampled
      });
    }
})
```
{% endtab %}

{% tab title="Java" %}
```java
import io.opentelemetry.api.trace.Span;
import io.opentelemetry.api.trace.SpanContext;

Bugsnag bugsnag = new Bugsnag(this.bugsnagToken);
bugsnag.addCallback(report -> {
    Span currentSpan = Span.current();
    if(Objects.nonNull(currentSpan)) {
        SpanContext spanContext = currentSpan.getSpanContext();
        report.addToTab("Aspecto", "trace_id", spanContext.getTraceId());
        report.addToTab("Aspecto", "workspace_id", "optional workspace id");
        report.addToTab("Aspecto", "sampled", spanContext.getTraceFlags().isSampled());
    }
});
```
{% endtab %}

{% tab title="Python" %}
```python
from opentelemetry import trace
import bugsnag

def get_active_trace_context():
    current_span = trace.get_current_span()
    if current_span != None:
        span_context = current_span.get_span_context()
        return trace.format_trace_id(span_context.trace_id), span_context.trace_flags.sampled

def callback(event):
    trace_id, sampled = get_active_trace_context()
    event.add_tab("aspecto", {
        "trace_id": trace_id,
        "sampled": sampled,
        "workspace_id": "optional workspace id"
    })

# Call `callback` before every event
bugsnag.before_notify(callback)=
```
{% endtab %}

{% tab title="GO" %}
```go
import (
    "go.opentelemetry.io/otel"
    "github.com/bugsnag/bugsnag-go"
)

traceCtx := trace.SpanContextFromContext(ctx)
bugsnag.Notify(err, ctx, bugsnag.MetaData{
	"Aspecto": {
		"trace_id":     traceCtx.TraceID().String(),
		"workspace_id": "optional workspace id",
		"sampled":      traceCtx.IsSampled(),
	},
})
```
{% endtab %}

{% tab title=".NET" %}
```java
using System.Diagnostics;

public (string, bool) GetActiveTraceContext() {
    var currentActivity = Activity.Current;
    if (currentActivity == null){
        return (null, false);
    }
    var sampled = currentActivity.ActivityTraceFlags.Equals(ActivityTraceFlags.Recorded);
    return (currentActivity.TraceId.ToString(), sampled);
};


var (traceId, sampled) = GetActiveTraceContext();
if (traceId == null){
    return;
}
var bugsnag = new Bugsnag.Client(new Configuration("your-api-key-goes-here"));
bugsnag.BeforeNotify((Report report) =>
{
    report.Event.Metadata.Add("Aspecto", new Dictionary<string, object> { 
        {"trace_id", traceId},
        {"workspace_id", "optional workspace id},
        {"sampled", sampled},
    });
}
```
{% endtab %}

{% tab title="Ruby" %}
```ruby
require 'opentelemetry'

def get_trace_context
    current_span = OpenTelemetry::Trace.current_span
    if current_span
        sampled = current_span.trace_flags == OpenTelemetry::Trace::TraceFlags::SAMPLED
      return current_span.context.hex_trace_id, sampled
    end
    return nil
end  

Bugsnag.configure do |config|
  config.add_on_error(proc do |event|
    traceId, sampled = get_trace_context()
    event.add_metadata(:Aspecto, {
      trace_id: traceId,
      sampled: sampled,
      workspace_id: "optional workspace id", # Optional, will use default if not provided
    })
 
  end)
end
```
{% endtab %}
{% endtabs %}
