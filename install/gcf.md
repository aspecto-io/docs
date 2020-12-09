---
description: Instrument Google Cloud Function
---

# Install on GCF

Aspecto supports instrumenting Google Cloud Functions \(GCF\) using an **HTTP trigger**.  
To do so, [set up Aspecto](./#usage) as you'd usually do, and extract the `gcf` utility:

```javascript
const { gcf } = require('@aspecto/opentelemetry')();
```

Next, wrap your function handler definition with the returned utility.   
Example:

```javascript
// Before
exports.myEndpoint = (req, res) => { ... };

// After
exports.myEndpoint = gcf((req, res) => { ... });
```

If your is an express app, you can wrap it with the `gcf` utility as well.  
  
**Notice**: if your GCF is not deployed with a `package.json` file, make sure to provide the `packageName` option when initializing Aspecto.

