---
description: Instrument Aspecto on Google Cloud Functions
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

If your export is an express app, you can still wrap it with the `gcf` utility, like this:

```javascript
const { gcf } = require('@aspecto/opentelemetry')();
const express = require('express');

const app = express();
app.use('/users/:id', getUser);
app.use('/users/', getAllUsers);

// Set your GCF handler to your Express app and wrap it with the "gcf" utility.
exports.users = gcf(app);
```

  
**Note**: if your GCF is not deployed with a `package.json` file, make sure to provide the `packageName` option when initializing Aspecto.

