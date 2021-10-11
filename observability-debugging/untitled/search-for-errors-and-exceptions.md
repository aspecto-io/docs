# Search for Errors and Exceptions

The Trace Search tool can be used to filter and find errors or exceptions that may be detrimental to your application's success. 

In this guide, we'll go over how to view a list of every trace that contained an error, as well as how to observe and pinpoint why that error occurred. 

The entire process can be completed in 2 steps:

1. Locate the trace
2. Pinpoint the problem 

### Step 1: Locate the Trace 

1. Click on the **Trace Search** tab to view every monitored trace in your system. 

![](<../../.gitbook/assets/Screen Shot 2021-09-28 at 6.02.19 PM (1).png>)

2\. Determine the time frame for your search. For example, if you'd like to review actions that took place within the last 24 hours, set the time frame to **Today**. 

![](<../../.gitbook/assets/Aspecto - Flows  (4) (1).png>)

3\. Filter the traces by marking **Error** as one of the search parameters. 

![](<../../.gitbook/assets/Aspecto - Flows  (5).png>)

You will now see a complete list of traces from the last 24 hours that contained an error. You can continue to refine your search using additional parameters or you can sort the search results according to _Flow Length_, _Execution Time_, and more. 

You can also refine your search using the displayed graph above the list of traces. The x-axis represents the execution time for each trace and the y-axis represents the function. By clicking on **function** or **group by**, you can alternate what the y-axis represents and can aggregate traces together in order to view more information. 

4\. Select any row in the list to view more information. 

Once you've selected the trace, you can begin to further investigate why it contained an error and did not perform. 

### Step 2: Pinpoint the Error 

There are 3 main sections you can utilize to further investigate why an error or exception took place in a trace: summary, diagram, and timeline.

The summary and the diagram provide excellent insight on the trace itself and how different components rely and communicate with each other. The timeline section really digs deep into how long each component in the trace took to perform. 

![](<../../.gitbook/assets/error\_1 (1) (1).png>)

You can select any component in the diagram or the timeline to learn more. For example, as displayed in the graph above and below, you can select the component that is marked FAILED. Once you've clicked on the component, its associated request, response, and dependencies will appear. You can now verify and check to see where something went wrong. 

![](<../../.gitbook/assets/error\_2 (1) (1).png>)

You can also click on the FAIL or EXCEPTION buttons directly to view the error message. 

![](<../../.gitbook/assets/error\_3 (1) (1).png>)

Now that you've pinpointed the problem, its time to debug and troubleshoot. 
