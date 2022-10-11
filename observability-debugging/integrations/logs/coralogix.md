# Coralogix

If you’re manage you logs within Coralogix, you can correlate logs with their corresponding traces and spans in Aspecto.

## Enrich Your Logs With Telemetry Data

To search for the logs related to a specific trace or the request transaction which generated the trace, you’ll need to enrich your logs with the trace ID and span ID. These log fields will let you drill down via a link that opens the specific trace in Aspecto from within Coralogix.

The enrichment process depends on the language and log type. You can find [here](../../../send-tracing-data-to-aspecto/aspecto-sdk/nodejs/customize-defaults/logs-correlation.md) example how to add it using Aspecto SDK.

## View logs from Trace within Aspecto <a href="#correlating-logs-and-traces" id="correlating-logs-and-traces"></a>

This integration allows you access and view your logs within Aspecto. It easily enables navigate from a trace to its correlated logs.

Integrate once, and start viewing your logs from the trace viewer . Here's how to integrate:

1\. Log into your **Coralogix** account as an administrator.

2\. Navigate to **Data Flow** > **API Keys**_._

![](<../../../.gitbook/assets/Screen Shot 2022-05-19 at 11.54.07.png>)

3\. Copy the __ **Logs Query Key** __ (generate new key if not exist)_,_ and enter it.

4\. Enter your **Elasticsearch-api**, following the next table [here](https://coralogix.com/docs/elastic-api/#how-to-query-your-coralogix-elastic-api) (e.g. `https://coralogix-esapi.coralogix.com:9443`).

5\. Enter the **trace ID** field name as you saved it in your logs (can be multiple fields, e.g. `traceId` if the field saved in the first level, or `metadta.trace-id` if the field name is `trace-id` within object called `metadta`).

6\. Enter the **span ID** field name as you saved it in your logs (can be multiple fields, e.g. `spanId`).

7\. Click on **Integrate.**

Once you complete the Integration, You will be able to view your logs correlated to specific trace (if exist) from the trace viewer.

![Navigate from a trace to its correlated logs](../../../.gitbook/assets/Logs-metrics-integration-w-warning-compressed.gif)

## View Trace in Aspecto from Coralogix Log <a href="#correlating-logs-and-traces" id="correlating-logs-and-traces"></a>

1. Once the trace ID is part of the log attributes, open the Coralogix Logs screen page.
2. Click on **Settings** in the top right corner and then on **Manage Actions** Configurations at the bottom.

![](<../../../.gitbook/assets/Screen Shot 2022-04-10 at 17.06.09.png>)

3\. A pop-up window will be displayed. It contains the list of defined **Actions** or if the list is empty it will open a new **Untitled Action**.

![](<../../../.gitbook/assets/Screen Shot 2022-04-10 at 17.19.56.png>)

4\. Click on the **+ADD NEW ACTION** button to define a new **Action** if it is not the first action.

5\. Name the new **Action** within the **Untitled Action** pop-up.

6\. In the URL field add the url for the trace in Aspecto.

When you start typing the curly brackets in **\{{** **\}}** you will see the hints list. There are 3 types of variables:

* $g – general variables which can be used with any log (START\_DATE, END\_DATE, SELECTED\_VALUE)
* $m – metadata variables: Coralogix metadata fields (applicationName, subsystemName, severity, etc)
* $i – any indexed field

For example:&#x20;

```
https://app.aspecto.io/app/traces/{{$d.contextTraceId}}
```

7\.  In the Advanced option you can make the **Action** available only for specific Applications and/or Subsystems and also you can share it with your team mates by clicking on **Make Public when saved**.\


![](<../../../.gitbook/assets/Screen Shot 2022-04-10 at 17.47.10.png>)

8\. Click on the Reset button to clear the entered data or on the **SAVE ACTION** button to save it.

### Jump from Log to Trace in Aspecto

After you added the trace ID to your Coralogix logs and defined the action, you are now able to jump easily from the log to its correlated trace in Aspecto.

1. Open the Logs page and define your query.
2. Trigger the **Action** by:
   * Clicking on (…) next to the specific log or on the JSON key. Only **Actions** are visible which don’t contain “$g.SELECTED\_VALUE” variable in the URL definition)
   * Clicking on a value will list all defined **Actions** (including **Actions** which contain “$g.SELECTED\_VALUE” variable in the URL definition). If the **Action** is defined with “$g.SELECTED\_VALUE” then the value from that JSON key will be passed.

![](<../../../.gitbook/assets/Screen Shot 2022-04-10 at 18.00.48.png>)

![](<../../../.gitbook/assets/Screen Shot 2022-04-10 at 18.01.01.png>)

3\. Once the **Action** is chosen, the defined URL will be open in new tab.
