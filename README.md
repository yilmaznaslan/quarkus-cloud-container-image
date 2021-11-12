# Quarkus/Cloud/container-image Project

## Introduction
This projects uses the Quarkus Framework to build a simple application and containrize it<br>
Steps to follow;

1. Download the application 
2. Build the .jar file and push it to `docker.io` container repository



## Dependencies
Project uses the following dependencies;
- `quarkus-config.yaml`(optional)
    - It is needed to configure the application using the YAML format. You can also use application.properties
- `quarkus-container-image-docker`

 ```
<dependency>
    <groupId>io.quarkus</groupId>
    <artifactId>quarkus-config-yaml</artifactId>
</dependency>

<dependency>
    <groupId>io.quarkus</groupId>
    <artifactId>quarkus-kubernetes</artifactId>
</dependency>

<dependency>
    <groupId>io.quarkus</groupId>
    <artifactId>quarkus-container-image-docker</artifactId>
</dependency>
```

## Configuratation

### 1. Configuring the `quarkus-container-image-docker` extension

Update the `registery`, `group` and `builder` fields. If you want more information about configuring the `quarkus-container-image-docker` dependency check [here]().


```
quarkus:
  container-image:
    builder: docker
    registery: docker.io
    group: yilmaznaslan
    name: quarkus-${quarkus.application.name:unset}
    tag: ${quarkus.application.version:latest}
    build: true
    push: true

```
This config will automatically build and push after each compilation process.

### 2. Configuring the `quarkus-kubernetes` extension
The Quarkus `quarkus-kubernetes` extension 
- enables to automatically generate Kubernetes **resources/manifests** based on user-supplied configuration.
- can deploy the application to a target Kubernetes cluster by applying the generated manifests to the target cluster’s API Server
- When either one of **container image extensions** is present, Quarkus has the ability to create a container image and push it to a registry **before** deploying the application to the target platform.

```
quarkus:
  kubernetes:
    image-pull-policy: always # ifNotPresent
    service-type: NodePort #ClusterIp, NodePort
    namespace: default
    replicas: 1
```

## Installation

### 1. Run the application in dev mode and test it

You can run your application in dev mode that enables live coding using:
```
./mvnw compile quarkus:dev
```

Go and check the application at http://localhost:8080/hello

### 2. Pack the application and push it to a container repository

The application can be packaged using:

```
./mvnw clean package
```
It produces the `quarkus-run.jar` file in the `target/quarkus-app/` directory.


## Reference
- [Quarkus Guide / Container Image](https://quarkus.io/guides/container-image)
- [Quarkus Guide / Deploying to kubernetes](https://quarkus.io/guides/deploying-to-kubernetes)