# About Observability

## Observability

Observability is the ability to understand system behavior and state by looking at its output. In this context, the output data is usually referred to as 'telemetry data', and there are three major well-used categories: logs, metrics, and traces.\
\
Aspecto uses a method called **distributed tracing** to increase the observability of your system and produce insights for pre-production and production environments.

## Tracing

A "trace" is a collection of operations, triggered as a result of a single logical event. A "span" describes an operation that happened in the system, the context in which it occurred, and relevant attributes. Later we will dive into the composition of a trace.\
\
Tracing is very powerful, because it allows you to investigate some types of issues with your system more efficiently than with simple logging and metrics.\
\
This is an example of what a trace looks like in Aspecto:

![](<../.gitbook/assets/Diagram and timeline.png>)

The diagram and timeline above shows how an HTTP request was handled internally:

* It first triggered an HTTP call to route '/user/token' on service 'user-service' which performed the FIND\_ONE operation on the 'aspecto-demo' database and returned status code 200 to the caller.
* Then, it sent an HTTP GET request to an external API (`en.wikipedia.org`) which returned status code 200.
* After that, 3 messages were sent to the SQS messaging queue.

This diagram provides the context and understanding you need in order to build and debug your code faster.

Now that we explained what a trace looks like in Aspecto, let's talk a little about the elements that comprise a trace:

#### Operations

In the example above, each bullet is an operation: "incoming HTTP request was handled", "database query was executed", "message sent into queue". Operations or 'spans' are meaningful events, usually I/O, that occur in the system.&#x20;

#### Operation Context

Operation context means that operations can be linked to one another in a child-parent relation and grouped to represent a single transaction or handling of one logical event by the system:

* "executing this MongoDB query was part of handling HTTP request on that route"
* "incoming HTTP request to this endpoint triggered those outgoing HTTP requests"

#### Operation Attributes

Each operation contains attributes which give additional info about what happened, for example:

* **HTTP**: URL, method, status code.
* **Database**: table name, query.
* **Messaging System**: system type, topic name, message id.

#### Auto Instrumentation

You don't need to do anything except importing Aspecto and initializing it with a token. All the rest is handled with automatic code 'instrumentation'.\
\
Basically, auto instrumentation means that integration with the actual packages you use in your application, collecting the trace, and extracting attributes for each operation is done automatically.

Our SDK automatically collects operations from common Node.js microservices packages:

* http/https
* express
* mongoose/mongodb
* ioredis
* aws-sdk
* kafkajs
* amqplib (RabbitMQ)
* typeorm
* Sequelize
* neo4j-driver
* @elastic/elasticsearch

Each time your application invokes a function from one of these libraries, for example`http.get('http://users-service/user/1234', cb)`, the relevant open-telemetry plugin runs and records this function call into a span**.** The recorded data includes, among other things:

* Timing: operation start time and duration.
* ID of the span itself and its parent (which is used later to connect relevant spans into a tree).&#x20;
* Attributes relevant to this operation.

Some of the auto-instrumentation plugins were developed by Aspecto and are available as open-source,\
check them out here: [https://github.com/aspecto-io/opentelemetry-ext-js](https://github.com/aspecto-io/opentelemetry-ext-js)

#### Distributed Tracing

Context works well in a distributed environment, connecting related operations which span **multiple microservices** into a single trace. This feature helps reveal how an event in one service is propagated in the system and what is triggered in downstream services.&#x20;

For instance, in the earlier example shown in the diagram, it is possible to tell that the MongoDB query on collection "users" from "user-service", was originally initiated by an HTTP call in a different service, "scraper-service" and includes the specific route.

Aspecto will show you interactions between your microservices, either synchronously - HTTP, or asynchronously - via messaging systems such as Kafka, RabbitMQ, SQS.

## OpenTelemetry

Aspecto uses [OpenTelemetry](https://opentelemetry.io/docs/), which is an open-source framework by CNCF, backed and maintained by industry-leading companies.

