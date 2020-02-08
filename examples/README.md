# example

The following example will show off the usage of patron involving all components implemented.
The processing will be kicked of by sending a request to the HTTP component. The flow then will be the following:

## Service 1

- Accepts HTTP JSON request from curl
- Sends HTTP protobuf request to service 2
- Responds to curl

## Service 2

- Accepts HTTP protobuf request from service 1
- Publishes a message to Kafka
- Responds to service 1

## Service 3

- Consumes a message from service 2 via Kafka
- Publishes a message to AMQP

## Service 4

- Consumes a message from service 3 via AMQP
- Publishes a message to AWS SNS linked to an SQS queue

## Service 5

- Consumes a message from service 4 via AWS SQS
- Send a rpc request to gRPC server of service 5 and logs response

## Service 6

- Receives a request from service 5
- Responds to service 5

Since tracing instrumentation is in place we can observer the flow in jaeger.

## Prerequisites

- Docker
- Docker compose

## Setting up environment

To run the full example we need to start [jaeger](https://www.jaegertracing.io/) and [prometheus](https://prometheus.io/). We can startup both of them using docker-compose with the following command.

```shell
docker-compose up -d
```

To tear down the above just:

```shell
docker-compose down
```

## Running the examples

Start first service:

```shell
go run examples/first/main.go
```

Start second service:

```shell
go run examples/second/main.go

```

Start third service:

```shell
go run examples/third/main.go
```

Start fourth service:

```shell
go run examples/fourth/main.go
```

Start fifth service:

```shell
go run examples/fifth/main.go
```

Start sixth service:

```shell
go run examples/sixth/main.go
```

and the use curl to send a request:

```shell
curl -d '{"Firstname":"John", "Lastname": "Doe"}' -H "Content-Type: application/json" -X POST http://localhost:50000
```

After that head over to [jaeger](http://localhost:16686/search) and [prometheus](http://localhost:9090/graph).