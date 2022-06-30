# Troubleshoot in Your Local Environment

Visualizing your application's traces is a powerful way to deeply understand and debug your microservices.&#x20;

Using the **Aspecto Live Stream Traces** tool, easily view real-time traces, through different endpoints and microservices based on the local traffic you choose to send. You can use this tool to analyze your traces, view microservice dependencies, and understand how a code change affected your distributed system.

## Getting Started with Live Stream Traces

The Aspecto SDK can be installed within the microservices in your local environment. Once you've installed the SDK, every time data is locally created on your microservice, it will appear within the Aspecto UI.

Once the process begins, a link will be outputted. Click on the link to open our **Live Stream Traces** mode and view traces from every microservice that is running in your environment.&#x20;

```
=====================================================================================================================================
|                                                                                                                                   |
| 🕵️‍♀️See the live tracing stream at https://app.aspecto.io/app/live-traces/sessions?instanceId=14243e72-14dc-4255-87af-ef846b247578   |
|                                                                                                                                   |
=====================================================================================================================================
```

Once traffic begins to trace you will see a list of all your traces displayed directly within the Aspecto Live Trace tool. The traces are streamed and automatically updated based on the local traffic you choose to send. Simply click on any row to analyze the relevant trace.&#x20;

![Live Stream Traces Mode](<../.gitbook/assets/Screen Shot 2022-06-20 at 18.02.41.png>)

