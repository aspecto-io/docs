---
description: Correlate your logs with Aspecto traces.
---

# Logs Correlation

A common use case for flow search is to see the related flow while inspecting a log event.  
You can do this with Aspecto. To do this, attach the active **traceId** to your logs.

## Setup

Follow these steps to attach traceId.

Use the `getContext` method, exposed from our package:

```javascript
const { getContext } = require('@aspecto/opentelemetry');

console.log('Something happened!', { traceId: getContext().traceId });
```

If you're using a custom logger, it's recommended to add the traceId to all logs by default.  
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

## Use

After completing the setup, and deploying your service, find the relevant log in your logging solution \(i.e. CloudWatch, logz.io, etc...\) and copy the traceId.

![Finding the traceId in a CloudWatch log](../.gitbook/assets/image%20%287%29.png)

Next, go to [Flow Search](flow-search.md) and search for the traceId you just copied:

![Searching for traceId in Flow Search](../.gitbook/assets/image%20%288%29.png)

You should be able to see and debug the relevant flow.

