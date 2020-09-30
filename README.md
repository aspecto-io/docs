# Aspecto Documentation

## How to install Aspecto's SDK for nodejs

Aspecto is using [OpenTelemetry](www.opentelemetry.io) to collect data, you can install it by:

```
$ npm install @aspecto/opentelemetry
```

### Usage

In the root folder create an `aspecto.json` file with content `{"token" : "-- token goes here --"}`. You can get your token from [here](https://app.aspecto.io/app/integration)

Add this call at the top of your app entry point:

```text
require('@aspecto/opentelemetry')();
```

### Configuration

You can pass the following configuration to the aspecto client:

| Option | Type | Description |
| :--- | :--- | :--- |
| env | string | set environment name manually instead of using `env` environment variable |
| aspectoAuth | UUID | set aspecto token from code instead of using `aspecto.json` |
| packageName | string | set packageName manually instead of reading it from `package.json` |
| packageVersion | string | set packageVersion manually instead of reading it from `package.json` |
| local | boolean | when set to true, enable [live flows](https://www.npmjs.com/package/@aspecto/opentelemetry#live-flows) |
| isolate | boolean | when set to true, enable isolated mode for [live flows](https://www.npmjs.com/package/@aspecto/opentelemetry#live-flows) |
| liveExporterPort | number | specify port for [live flows](https://www.npmjs.com/package/@aspecto/opentelemetry#live-flows) |
| logger | logger interface | logger to be used in this tracing library. common use for debugging `logger: console` |
| customZipkinEndpoint | URL | Send all traces to additional Zipkin server for debug |

### Live Flows

Live Flows captures all payloads and traces in your local environment and automatically extract the topology & dependencies between endpoints. You can activate it using `{local:true}`, like so:

```text
require('@aspecto/opentelemetry')({    local: true,});
```

Live flows can operate in 2 modes:

* connected
* isolated

#### Connected mode

This is the default mode. It allows you to capture flows from all the microservices that you're running locally \(both on the host env and docker\) with `local` mode enabled. Once the process starts it will output the following link:

```text
=====================================================================================================================================
|                                                                                                                                   |
| 🕵️‍♀️See the live tracing stream at https://app.aspecto.io/app/live-flows/sessions?instanceId=14243e72-14dc-4255-87af-ef846b247578   |
|                                                                                                                                   |
=====================================================================================================================================
```

You only need to click the link once to see traces from all the microservices, that are running on your environment and have local mode enabled. Also this link is valid for a limited period of time \(couple of days, but it may change in the future\). If you don't see trace from some microservice \(or none of them\), please click the newly-generated link.

#### Isolated mode

In this mode you can only see flows from one microservice. Also, in this case, all the data is being sent directly to the browser. To activate isolated mode use `isolate` option like so:

```text
require('@aspecto/opentelemetry')({local: true,isolate: true});
```

In isolated mode, the message in the console will look like this \(with `port` parameter\):

```text
===============================================================================================
|                                                                                             |
| 🕵️‍♀️ See the live tracing stream at https://app.aspecto.io/app/live-flows/sessions?port=59778 |
|                                                                                             |
===============================================================================================
```

Note: In case the Live Flows port keep on changing, just add an environment variable `ASPECTO_LIVE_PORT=59778` In order for live flows to work in isolated mode when running the service inside of the container the port that is used by live flows has to be published \(https://docs.docker.com/config/containers/container-networking\).  


