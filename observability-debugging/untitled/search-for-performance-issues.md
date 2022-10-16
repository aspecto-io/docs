# Search for Performance Issues

Performance issues almost always lead to customer frustration and dissatisfaction, especially when your application isn't performing as it should or it is too slow. The Trace Search tool can be used to easily locate these performance issues within your distributed system and pinpoint exactly what component is causing the delay or extended execution time.&#x20;

In this guide, we'll go over how to use Trace Search to find time-related performance issues and how you can use the Aspecto diagram and Timeline hierarchy to define the exact component that's causing the delay.&#x20;

The entire process can be completed in 2 steps:

1. Locate the trace
2. Pinpoint the problem&#x20;

### Step 1: Locate the Trace&#x20;

1. Navigate to the **Trace Search** tab to view every monitored trace in your system.&#x20;

![](<../../.gitbook/assets/Trace search playground.png>)

2\. Determine the time frame for your search. For example, if you'd like to review actions that took place within the last 24 hours, set the time frame to **Last 24 hours**.&#x20;

![](<../../.gitbook/assets/Trace search last 24h.png>)

3\. Sort the traces by duration by clicking on the **Duration** column title.&#x20;

You will now see a complete list of traces from the last 24 hours, displayed from longest  to shortest execution time. You can continue to refine your search as well using the parameters available on the left-side pane.&#x20;

You can also refine your search using the displayed graph above the list of traces. The x-axis represents the execution time for each trace and the y-axis represents the function. By clicking on **function** or **group by**, you can alternate what the y-axis represents and can aggregate traces together in order to view more information.&#x20;

4\. Select the very first row displayed in the list. This trace took the longest to complete.&#x20;

Once you've selected the trace, you can begin to further investigate why it took so long to perform.&#x20;

### Step 2: Pinpoint the Problem&#x20;

There are 3 main sections you can utilize to further investigate why a specific action takes so long to perform: summary, diagram, and timeline.&#x20;

While the summary and diagram provide excellent insight on the trace and how different components rely and communicate with each other, the timeline section really digs deep into how long each component in the trace took to perform.&#x20;

![](<../../.gitbook/assets/Trace Viewer2.png>)

Taking the image above as an example, the timeline pinpointed exactly which components took much longer to perform than the rest. You can click on the component itself within the timeline to understand why this was the case.&#x20;

![](<../../.gitbook/assets/Span details 2.png>)

Once you've selected the component, the request, response, and dependencies will appear. You can now verify and check to see if something is wrong in the request, causing the response time to delay.&#x20;

###
