---
description: Use Aspecto to uncover flows and visualize data in production environments
---

# Setting Up

You can use Aspecto in production environments, to improve your debugging capabilities. You can use the  following:

* Search for flows, and then examine them for details:

{% page-ref page="flow-search.md" %}

* See flows related to log events you are examining:

{% page-ref page="logs-correlation.md" %}

## Configure for Deployed Environments

To view flows in production environments, follow [these steps](../getting-started/install/#configuration) to instrument your application to send opentelemetry information to Aspecto. When activating, set`{local:false}:`

```javascript
require('@aspecto/opentelemetry')({ local: false });
```

Aspecto uses information from `.git` folder to extract a `gitHash` of the service that is being instrumented. If you don't have this folder in your docker container, you can specify `gitHash` manually by setting `ASPECTO_GITHASH` environment variable.   
Otherwise, flows for those services will be displayed with the version equal to `not-set`.

It is also important to have an environment name \(production/staging/etc\).   
The SDK uses the `NODE_ENV` environment variable by default, but you can also specify it manually with the [env config option](../getting-started/install/#configuration).   
If neither option or environment variable are specified, filtering by environment won't work, since all flows will be under the same environment \(equal to `not-set`\).











