# Celery

Golang client library for executing **[Celery](http://www.celeryproject.org)** tasks.

It currently support AMQP brokers by using http://github.com/streadway/amqp

[![GoDoc](https://godoc.org/github.com/ulule/celery?status.png)](https://godoc.org/github.com/ulule/celery)

## Installation

Using [dep](https://github.com/golang/dep)

```console
dep ensure -add github.com/ulule/celery@master
```

or `go get`

```console
go get -u github.com/ulule/celery
```

## Usage

Use http://github.com/streadway/amqp and open a connection and get a channel.

```go
import (
	"github.com/ulule/celery"
	"github.com/streadway/amqp"
)

func main() {
	conn, err := amqp.Dial("amqp://guest@localhost://")
	if err != nil {
		panic(err)
	}
	defer conn.Close()

	ch, err := conn.Channel()
	if err != nil {
		panic(err)
	}

	task, err := celery.NewTask("tasks.test", []string{}, nil)
	if err != nil {
		panic(err)
	}

	err = task.Publish(ch, "", "celery")
	if err != nil {
		panic(err)
	}
}
```
