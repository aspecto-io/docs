---
description: Instrument Aspecto in AWS Lambda
---

# Install on AWS Lambda

Aspecto supports instrumenting AWS Lambdas.  
To do so, [set up Aspecto](./#usage) as you'd usually do, and extract the returned `lambda` utility:

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

**Note:** if your lambda is not deployed with a `package.json` file, make sure to provide the `packageName` option when initializing Aspecto.  


