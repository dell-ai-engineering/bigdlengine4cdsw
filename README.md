# DellEMC BigDL and Analytics Zoo Engine for Cloudera Data Science Workbench

## Overview
This repository contains preconfigured engines for the Dell EMC Ready Solution for AI - Machine Learning with Cloudera Hadoop. There are two engines. One is configured with [Intel BigDL](https://bigdl-project.github.io/master/#whitepaper/) a distributed deep learning library for Apache Spark. The other is configured with [Analytics Zoo](http://analytics-zoo.github.io/) a unified analytics + AI platform for Spark. Jumpstart examples are located in the [BigDL4CDSW](https://github.com/dell-ai-engineering/BigDL4CDSW) repository.

### Versions
- BigDL 0.7.0
- Analytics Zoo 0.3.0
- Spark 2.2
- Scala 2.11.8
- Java 8

Note: The Dockerfile to build these engines use BigDL and Analytic Zoo builds that are specific for above versions of Spark and Scala. If your environment has other versions of Spark, edit the Dockerfile accordingly. 

#### Note on Java 7
Intel BigDL recommends Java 8 when using Spark 2.x as Java 7 may cause performance issues. If you are required to use Java 7 then follow the instructions to [build from source](https://bigdl-project.github.io/master/#ScalaUserGuide/install-build-src/#download-bigdl-source) using an environment with Java 7. Then replace the /opt/Intel directory with the Java 7 compiled BigDL and edit the spark.jars in spark-defaults.conf file if using the Jumpstart templates.

## How to use Dockerfile for BigDL and Analytics Zoo

Download this repository to your workstation. Change the working directory to either the BigDL or analytics-zoo based on the engine you want to deploy.

#### Build image and push to a Docker repository
    sudo yum install docker
    sudo systemctl start docker

#### Run a docker registry: 
Run registry container using the following command. This assumes the certificate is in a certs directory under the dockerfile. If you already have a docker registry, you can ignore this step and proceed to the next.
```
docker run -d \
  --restart=always \
  --name <registry-name> \
  -v `pwd`/certs:/certs \
  -e REGISTRY_HTTP_ADDR=0.0.0.0:443 \
  -e REGISTRY_HTTP_TLS_CERTIFICATE=/certs/domain.crt \
  -e REGISTRY_HTTP_TLS_KEY=/certs/domain.key \
  -p 443:443 \
  registry:2
```
Change <registry-name> to a desired name for your docker registry.

A docker registry is secured using TLS and requires a certificate. [Docker registry configuration guide](https://docs.docker.com/registry/deploying/). If you must use a self signed certificate follow the [Docker guide for an insecure registry server](https://docs.docker.com/registry/insecure/#use-self-signed-certificates)

#### Build the container
Build the container for BigDL
```
    docker build --network=host -t <registry-name>/bigdl:0.7.0 . -f Dockerfile
```

Build the container for Analytics Zoo
```    
    docker build --network=host -t <registry-name>/analytics-zoo:0.3.0 . -f Dockerfile
```

Change the docker repo from ```<registry-name>``` to docker registry name.

#### Test that it works
```
    docker run -it <registry-name>/bigdl:0.7.0 /bin/bash
```
OR
```
    docker run -it <registry-name>/analytics-zoo:0.3.0 /bin/bash
``` 
You can exit out of the container by typing 'exit'.

#### Push the container image
```
    docker push <registry-name>/bigdl:0.7.0
```
OR
```
    docker push <registry-name>/analytics-zoo:0.3.0
```

### Add the engine to CDSW
1. Log in to CDSW as a site administrator
2. Go to Admin and then Engines
3. Name the engine and add the docker registry location

### Verify that the engine works
1. Log in to CDSW
2. Create a new project
3. Copy the spark-defaults.conf file from the respository to the root folder of the CDSW project 
4. Open a new workbench session
5. Before starting the workbench change the selection to the new BigDL engine or Analytics Zoo engine
6. Start the engine by opening a workbench


### spark-defaults.conf
For both BigDL and Analytic Zoo, sample spark-defaults.conf files are provided in the corresponding folder. These files need to be copied to the root directory of each CDSW project. The files specifies the Spark parameters for the project. The parameters must be edited to suite the requirements for the project

