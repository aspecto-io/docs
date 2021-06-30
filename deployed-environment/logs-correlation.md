---
description: Correlate logs from your flows with Aspecto traces.
---

# Logs Correlation

A common use case for flow search is to see the related flow while inspecting a log event.  
You can do this with Aspecto. To do this, you must attach the active **traceId** to your logs.

## Setup

Follow these steps to attach **traceId** to your logs.

Use the `getContext` method, exposed from our package:

```javascript
const { getContext } = require('@aspecto/opentelemetry');

console.log('Something happened!', { traceId: getContext().traceId });
```

If you're using a custom logger, we recommend you add the **traceId** to all logs by default.  
Here's an example using **winston**:

```typescript
const { getContext } = require('@aspecto/opentelemetry');
const winston = require('winston');

const addAspectoTraceId = winston.format((info) => {
  info.traceId = getContext().traceId;
  return info;
});

const logger = winston.createLogger({
  format: winston.format.combine(
    addAspectoTraceId(), 
    winston.format.json()
  ),
  transports: [ ... ],
});
```

### Sampled Flag

Some traces may be unsampled due to [sampling rules](../settings/sampling-rules.md) or  the `samplingRatio` SDK setting.  
In those cases, you may want to reflect this on your log, so later on, you'd be able to know whether to expect to [see the matching trace on Aspecto](../logging.md), or not.  
  
For this, the `getContext()` response payload will also contain a boolean called `sampled`, reflecting the sampling result.

## Use

After completing the setup, and deploying your service, find the relevant log in your logging solution \(e.g., CloudWatch, logz.io, etc...\) and copy the **traceId**.

![Finding the traceId in a CloudWatch log](../.gitbook/assets/image%20%287%29.png)

Next, go to [Flow Search](../flow-search.md) and search for the traceId you just copied:

![Searching for traceId in Flow Search](../.gitbook/assets/image%20%288%29.png)

Then, visualize and debug the flow.

