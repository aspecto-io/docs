---
description: How to use Aspecto Live Flow to view services running locally
---

# Live Flows Overview

The Live Flow is a view of the data flows occurring in your services, starting with your local traffic. It shows in near real-time the flows through different endpoints and microservices, based on traffic you send it.

You can use this view to analyze the data flows, discover dependencies, and detect the effect of development changes.

## Getting started with Live Flow

To get started with Live Flow, follow [these steps ](../install.md#configuration)to instrument your application for the telemetry that Aspecto uses. This securely sends information about live data flows to Aspecto, which is then shown in the Live Flow view.

Follow the [link](https://docs.aspecto.io/v1/install#connected-mode) shown in your terminal, to open the Live Flow in your browser.

The initial view is waiting for live traffic to your service.

![](../.gitbook/assets/whatsapp-image-2020-10-20-at-14.44.01.jpeg)

Now, send traffic to one or more endpoints in the instrumented service \(for example from Postman\). Our SDK will capture this and update the Live Flow view with the flows that were detected.

## Live Flow UI

The Live Flow view shows the  following:

![](../.gitbook/assets/whatsapp-image-2020-10-18-at-14.51.42%20%282%29.jpeg)

**1** \(top\) - a list of the flows detected

**2** \(lower left\) - graph view of a selected flow  

**3** \(lower right\) - information view of a selected node in the graph



## Flows Table

The list of flows shows the service and entry point for the flow, the time, and the number of nodes involved.

The list of flows is updated as new flows are detected by the telemetry sent from the service.  
New flows are shown on top.

## Graph

The graph view shows the services and endpoints involved in a flow.  
- explain on the edges \(order, operation, status\)  
- explain on the label  
- explain on the flag

## Node information

The information view shows details for a node in the graph. This includes details for the message in the data flow \(request, response\).  
- explain different component show different details

There are also the following additional options in the  information view

## Search

You can search for text that appears in the listed data flows. For example, you can search for specific field names, or for values, in either the request or response. All flows that match the search text are highlighted.

## Additional Views

{% page-ref page="visualize-data-flows/timing.md" %}

{% page-ref page="visualize-data-flows/logging.md" %}

### 

