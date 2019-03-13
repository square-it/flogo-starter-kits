# Flogo application with OpenTracing

This is a minimal Flogo application based on the [builtin Flogo example app](https://github.com/project-flogo/core/blob/v0.9.0-alpha.6/examples/engine/flogo.json).

It adds the [*Square IT Services* OpenTracing contribution](https://github.com/square-it/flogo-opentracing-listener).

## Requirements

* [Go language 1.11+](https://golang.org)
* [Flogo CLI](https://github.com/project-flogo/cli)

## Create the application

The application can be created using the [*flogo.json* from this directory](./flogo.json) and the [Flogo CLI](https://github.com/project-flogo/cli):

```
flogo create -f https://raw.githubusercontent.com/square-it/flogo-starter-kits/master/opentracing/flogo.json flogo-app-opentracing
```

## Build the application

```
cd flogo-app-opentracing               # enter in the directory of the created application
flogo build -e                         # build the application and embed the flogo.json in the binary
```

## Test the application

In real-life scenarios, an OpenTracing collectors should be reachable through HTTP or Kafka.

To test this *getting started* application, a collector running in a local Docker container
(or even no collector at all) can be used.

### With an OpenTracing collector 

> using Zipkin in a local Docker container

Launch a Zipkin collector listening HTTP on port 9411:

```
docker run --name zipkin -d -p 9411:9411 openzipkin/zipkin
```

Configure the OpenTracing contribution:

```
export FLOGO_OPENTRACING_IMPLEMENTATION=zipkin
export FLOGO_OPENTRACING_TRANSPORT=http
export FLOGO_OPENTRACING_ENDPOINTS=http://127.0.0.1:9411/api/v1/spans
```

Replace *127.0.0.1* by the actual IP of the Zipkin collector.

### Without an OpenTracing collector

> using Jaeger stdout output to validate this contribution installation

Configure the OpenTracing contribution:

```
export FLOGO_OPENTRACING_IMPLEMENTATION=jaeger
export FLOGO_OPENTRACING_TRANSPORT=stdout
export FLOGO_OPENTRACING_ENDPOINTS=none
```

### In both cases

After configuring the contribution, run the application:

```
./bin/flogo-app-opentracing*           # * will catch the .exe if testing on Windows
```

Trigger a REST event with cURL:
```
curl http://127.0.0.1:8888/test/hello  # the REST endpoint path is "/test/:val"
```

## Going further

Full documentation can be found [in the OpenTracing contribution repository](https://github.com/square-it/flogo-opentracing-listener/blob/v0.0.2/README.md).
