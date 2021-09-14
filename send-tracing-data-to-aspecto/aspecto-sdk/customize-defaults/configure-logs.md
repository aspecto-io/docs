# Send Logs

## Set up logging in your application

In order to see log information, you must instrument your application with a logger.  
Add a logger when initializing the Aspecto telemetry:

```typescript
const Instrument = require('@aspecto/opentelemetry');
Instrument({
    local: true,
    logger: myLogger
});
```

Then, configure your logger to collect logs for the levels you are interested in.  
If your logger is initialized or changed later in your service lifecycle, you can also set the logger after initializing Aspecto, like this:

```typescript
const Instrument = require('@aspecto/opentelemetry');
const { setLogger } = Instrument({ local: true });

 // initialize your service ...
 setLogger(myLogger); 
```

{% page-ref page="logs-correlation.md" %}

