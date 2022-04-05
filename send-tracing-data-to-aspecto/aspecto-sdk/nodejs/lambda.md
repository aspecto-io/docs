---
description: Instrument your AWS Lambda using Aspecto
---

# For AWS Lambda

Aspecto supports instrumenting AWS Lambdas.\
There are 2 possible ways to do so:

### Option 1: Aspecto Wrapper File (recommended)

In your lambda src folder, create a file to initialize aspecto, such as `aspecto-wrapper.js` :

```javascript
// aspecto-wrapper.js example
const instrument = require('@aspecto/opentelemetry');

instrument({
    aspectoAuth: '*your-token-goes-here*',
    env: 'production',
    serviceName: 'your-lambda-name'
});
```

In your Lambda function configuration, add or update the `NODE_OPTIONS` environment variable to require the wrapper, e.g.:\
\
**`NODE_OPTIONS=--require aspecto-wrapper`**

### Option 2: Wrap Lambda handler with function

To do so, [set up Aspecto](https://docs.aspecto.io/v1/send-tracing-data-to-aspecto/aspecto-sdk) as you'd usually do, and extract the returned `lambda` utility:

```javascript
const instrument = require('@aspecto/opentelemetry');
const { lambda } = instrument(myConfig);
```

Next, wrap your function handler definition with the returned utility.

Example:

```javascript
// Before
module.exports.myCallbackHandler = (event, context, callback) => { ... };
module.exports.myAsyncHandler = async (event, context) => { ... };

// After
module.exports.myCallbackHandler = lambda((event, context, callback) => { ... });
module.exports.myAsyncHandler = lambda(async (event, context) => { ... });
```

**Note:** if your lambda is not deployed with a `package.json` file, make sure to provide the `serviceName` option when initializing Aspecto.\
