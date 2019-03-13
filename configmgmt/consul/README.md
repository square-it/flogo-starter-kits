# Flogo application with configuration in Consul

This is a minimal Flogo application based on the [builtin Flogo example app](https://github.com/project-flogo/core/blob/v0.9.0-alpha.6/examples/engine/flogo.json).

It adds the [*Square IT Services* Configuration Management contribution](https://github.com/square-it/flogo-config-mgmt).

## Requirements

* [Go language 1.11+](https://golang.org)
* [Flogo CLI](https://github.com/project-flogo/cli)

## Create the application

The application can be created using the [*flogo.json* from this directory](./flogo.json) and the [Flogo CLI](https://github.com/project-flogo/cli):

```
flogo create -f https://raw.githubusercontent.com/square-it/flogo-starter-kits/master/configmgmt/consul/flogo.json flogo-app-config-mgmt-consul
```

## Build the application

```
cd flogo-app-config-mgmt-consul        # enter in the directory of the created application
flogo build -e                         # build the application and embed the flogo.json in the binary
```

## Test the application

To test this *getting started* application, a Consul key-value store running in a local
Docker container will be used.

1. Start a single-host development-mode Consul agent with Docker (for testing purpose only)

```
docker run -d --name=dev-consul -e CONSUL_BIND_INTERFACE=eth0 -p 8500:8500 consul
```

2. Add a value for the property ```log.message``` in Consul

```
docker exec -t dev-consul consul kv put flogo/flogo-app-config-mgmt-consul/log/message "Consul message"
```

3. Run the application

```
./bin/flogo-app-config-mgmt-consul
```

4. In another terminal, trigger a flow:

```
curl http://127.0.0.1:8888/test/hello
```

You should see a log like:

```
INFO    [flogo.activity.log] -  Consul message
```

## Going further

Full documentation can be found [in the Configuration Management contribution repository](https://github.com/square-it/flogo-config-mgmt/blob/modules/consul/README.md).

