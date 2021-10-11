# Go

Instrument your Go app using openTelemetry and export traces to Aspecto's collector.



### Install OpenTelemetry SDK

```bash
go get go.opentelemetry.io/otel@v1.0.0 \
go.opentelemetry.io/otel/sdk@v1.0.0 \
go.opentelemetry.io/otel/trace@v1.0.0 \
go.opentelemetry.io/otel/sdk/resource \
go.opentelemetry.io/otel/propagation \
go.opentelemetry.io/otel/semconv/v1.4.0 \
go.opentelemetry.io/otel/exporters/otlp/otlptrace/otlptracehttp@v1.0.0
```

### Initialize OpenTelemetry

add to your `main.go` file the following imports

```go
import (
	"go.opentelemetry.io/otel"
	"go.opentelemetry.io/otel/exporters/otlp/otlptrace/otlptracehttp"
	"go.opentelemetry.io/otel/propagation"
	"go.opentelemetry.io/otel/sdk/resource"
	sdktrace "go.opentelemetry.io/otel/sdk/trace"
	semconv "go.opentelemetry.io/otel/semconv/v1.4.0"
)
```

and then in your main function add the following code

```go
	ctx := context.Background()
	traceExporter, err := otlptracehttp.New(ctx,
		otlptracehttp.WithEndpoint("otelcol.aspecto.io"),
		otlptracehttp.WithHeaders(map[string]string{"Authorization": "<YOUR ASPECTO TOKEN HERE>"}))

	if err != nil {
		log.Fatal(err)
	}

	res, err := resource.New(ctx,
		resource.WithAttributes(
			semconv.ServiceNameKey.String("<YOUR SERVICE NAME>"),
		),
	)

	if err != nil {
		log.Fatal(err)
	}

	tp := sdktrace.NewTracerProvider(
		sdktrace.WithSampler(sdktrace.AlwaysSample()),
		sdktrace.WithResource(res),
		sdktrace.WithBatcher(traceExporter))
	otel.SetTracerProvider(tp)
	otel.SetTextMapPropagator(propagation.TraceContext{})
```

## Auto Instrumentation

Go has support for a few popular libraries including `net/http` `go-redis` and `mongo-driver`

### net/http

`go get go.opentelemetry.io/contrib/instrumentation/net/http/otelhttp`

you will need to wrap each http handler with `otelHandler `see an example below

```go
handler := func(w http.ResponseWriter, req *http.Request) {
	//your handler code
}

http.Handle("/hello", otelhttp.NewHandler(http.HandlerFunc(handler), "Handler"))
```

### go-redis

`go get github.com/go-redis/redis/extra/redisotel/v8`

then add the following code to your `redis` client initialization code 

```go
import (
    "github.com/go-redis/redis/v8"
    "github.com/go-redis/redis/extra/redisotel"
)

rdb := rdb.NewClient(&rdb.Options{...})

rdb.AddHook(redisotel.NewTracingHook())
```

### mongo-driver

 `go get go.opentelemetry.io/contrib/instrumentation/go.mongodb.org/mongo-driver/mongo/otelmongo`

then add the following code to your mongo client initialization code 

```
	opts := options.Client()
	opts.Monitor = otelmongo.NewMonitor()
```

You can find the full list of instrumentations [here](https://opentelemetry.io/registry/?language=go\&component=instrumentation#)
