# Install on AWS Lamba

Aspecto supports instrumenting AWS Lambdas.  
To do so, [set up Aspecto](./#usage) as you'd usually do, and extract the returned `lambda` utility:

```javascript
const { lambda } = require('@aspecto/opentelemetry')();
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

**Notice**: if your lambda is not deployed with a `package.json` file, make sure to provide the `packageName` option when initializing Aspecto.  


