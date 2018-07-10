# DellEMC BigDL Engine for Cloudera Data Science Workbench

## Overview
This repository contains the preconfigured engine for the Dell EMC Ready Solution for AI - Machine Learning with Cloudera Hadoop. The engine engine is configured with [Intel BigDL](https://bigdl-project.github.io/master/#whitepaper/) a distributed deep learning library for Apache Spark.

## How to use
#### Build image and push to a Docker repository
    sudo yum install docker
    sudo systemctl start docker
#### Build the container
    docker build --network=host -t dellrepo/bigdl:0.5.0 . -f Dockerfile
#### Test that it works
    docker run -it dellrepo/bigdl:0.5.0 /bin/bash
#### Push the container image
    docker push dellrepo/bigdl:0.5.0

#### If you need to run a docker registry: 
Run registry container
```
docker run -d -p 5000:5000 --restart=always registry registry:2
```
Change the docker repo from dellrepo to \<hostname>:5000 when tagging images

### Add the engine to CDSW
1. Log in to CDSW as a site administrator
2. Go to Admin and then Engines
3. Name the engine and add the docker registry location

### Verify that the engine works
1. Log in to CDSW
2. Create a new project
3. Open a new workbench session
4. Before starting the workbench change the selection to the new BigDL engine
5. Start the engine
