---
description: Visualize the data flows in your apps and services
---

# Flow Visualization

Aspecto collects information on all **data flows** in your services. You view these flows on the Live Flow view.

Use this view to see details for all the data flows in your services, to help in debugging and troubleshooting your application, before deploying it to a mature environment.   
For each flow,  you can see the steps in the flow, starting with the initial endpoint. At each step, you can see the microservice, the endpoint involved, and extra details.

The example below shows a data flow, starting with an API request, traversing to a different microservice, and involving a database operation.

Click on the first stop in the flow after the entry flag \(highlighted in blue\), and see details of the http request and http response on the right.

![Flow starts from GET to &quot;wikipedia-service&quot;](../../.gitbook/assets/flow-wikipedia-service.png)

To visualize the next part of the flow, click on the next node in the chain.  
In this case, the next node is in a different microservice - user-service.

![](../../.gitbook/assets/flow-user-service.png)

The next step in the flow is an action on the MongoDB database, in this case, a _findOne_ operation.   
The query, along with other relevant data, such as the DB response, is shown on the right:

![](../../.gitbook/assets/flow-users-db.png)

The next sections explain how to use these Live Flow features:

* View timelines see the timing of each stage of a flow
* View logs to see raw data at various stages



