# Istio Operator

Istio Operator (a.k.a `istiops`) is a tool to manage traffic for microservices deployed via [Istio](https://istio.io/). It simplifies deployment strategies such as bluegreen or canary releases with no need of messing around with tons of `yamls` from kubernetes' resources.

## Architecture

<img src="https://github.com/pismo/istiops/blob/master/imgs/overview.png">

## Running tests

`$ go test ./... -v`

## Building the CLI

To use istiops binary you can just `go build` it. It will generate a command line interface tool to work with.

`./run` or `go build -o build/istiops main.go`

You can then run it as: `./build/istiops version`

## Commands on traffic shifting

### Each operation creates or removes items from both the VirtualService and DestinationRule

1. Clear all traffic rules, except for main one, from service api-gateway

`istiops traffic clear --label-selector app=api-gateway,build=1`

2. Send requests with HTTP header x-cid:seu_madruga to pods with labels: app=api-accounts,build=PR-10

`istiops traffic headers --headers x-cid=seu_madruga --label-selector app=api-accounts,build=PR-10`

3. Send 10% of traffic to pods with labels: app=api-gateway,version:1.0.0

`istiops traffic weight --percentage 10 --pod-selector app=api-gateway,version=1.0.0`

4. Removes all traffic (rollback), headers and percentage, for pods with labels: app=api-gateway,version:1.0.0

`istiops traffic rollback --label-selector --pod-selector app=api-gateway,version:1.0.0`

## Importing as a package

You can assemble `istiops` as interface for your own Golang code, to do it you just have to instanciate the needed struct-dependencies and call the interface directly. You can see examples at `./examples`