# Stanford CoreNLP Server - Dockerized Alternative

This repository is dedicated to expose and track a customized Docker Image of the traditional [Stanford CoreNLP server](http://stanfordnlp.github.io/CoreNLP/corenlp-server.html). This build intends to be used on personal or small research teams projects.

A ready-to-use [image](https://hub.docker.com/r/d1egoprog/stanford-corenlp) from Docker Hub is provided, along with the [deploy instructions](#deploy-alternatives) and the possibility of downloading and customizing the image through the Dockerfile using simple [build instructions](#build-alternatives). The assembled version is 4.1.0; however, it is possible to modify the `VERSION` variable via the Dockerfile.

Also, deployment testing is provided using plain `HTTP` via the `curl` command and the official python library [`stanza`](https://stanfordnlp.github.io/stanza/).

## Deploy Alternatives

To deploy the prebuilt docker image, two options are provided: use the `docker-compose` tool and the `docker` command from the CLI utility.

### Deploy using docker-compose

Download the prepared 'deploy.yml' file from the repository via `wget` and execute the command using the utility.

```
wget https://raw.githubusercontent.com/d1egoprog/stanford-corenlp-docker/main/deploy.yml
docker-compose -f deploy.yml up -d
```

### Deploy using Docker CLI

Directly run the 'docker' command like the following example. e.g., changing two variables. For more information, check the [Official Documentation](http://stanfordnlp.github.io/CoreNLP/corenlp-server.html)

```
docker run -e JAVA_XMX=12g -e ANNOTATORS=tokenize,ssplit,parse -p 9000:9000 d1egoprog/stanford-corenlp:4.1.0
```

## Testing the Installation

To check the functionality, you can open a web browser window to your docker-engine `IP` and the chosen service, e.g., `PORT=9000`, generally on [localhost:9000](http://localhost:9000). 

### Consuming by HTTP request

Also, to test your local or remote service, just send the following curl request into your preferred CLI.

```
curl --data 'The quick brown fox jumped over the lazy dog.' 'http://localhost:9000/?properties={%22annotators%22%3A%22tokenize%2Cssplit%2Cpos%22%2C%22outputFormat%22%3A%22json%22}' -o -
```

### Consuming by Library

To use the official python library [`stanza`](https://stanfordnlp.github.io/stanza/) a small example has been prepared in a jupyter notebook, [stanza-example](https://github.com/d1egoprog/stanford-corenlp-docker/blob/main/stanza-example.ipynb)

## Build Alternatives

For more information on the configuration and functionality of the [Stanford OpenNLP Server](https://stanfordnlp.github.io/CoreNLP/corenlp-server.html) use the official documentation.

### Build using Docker CLI

To build the image locally, clone the repository: 

```
git clone https://github.com/d1egoprog/stanford-corenlp-docker.git
```

Use the `docker` command CLI tool to build and run:

```
docker build -t stanford-corenlp:4.1.0 stanford-corenlp/.
docker run -p 9000:9000 stanford-corenlp:4.1.0
```

All the JVM parameters can be accessed by editing the Dockerfile and rebuilding the image by default; the parameters configured are:

```
ENV JAVA_XMX 8G
ENV ANNOTATORS all
ENV TIMEOUT_MILLISECONDS 60000
ENV THREADS 5
ENV MAX_CHAR_LENGTH 100000
ENV PORT 9000
```

If you do not want to edit the Dockerfile, environment variables can be overwritten from the `docker run` command, e.g., changing the JVM memory parameter `JAVA_XMX` to reserve more memory. 

```
docker run -e JAVA_XMX=12g -p 9000:9000 stanford-corenlp:4.1.0
```

### Build using docker-compose

If preferred, a docker-compose file is also available with the standard build from the docker file and an override configuration (same parameters from Dockerfile); this should be changed to set your desired annotators specific computing requirements. To run the service, run the command:

```
docker-compose -f build.yml up -d
```

Or use the compose file to build the Docker image, storing the following into a new `build.yml` file. Also is possible to override the variables, e.g., changing the JVM memory parameter `JAVA_XMX` to reserve more memory or changing `ANNOTATORS` task performed by the server. 

```
version: '3.7'

services:
  stanford_corenlp:
    image: d1egoprog/stanford-corenlp:4.1.0
    ports:
      - "9000:9000"
    environment: 
      - JAVA_XMX=12G
      - ANNOTATORS=tokenize
    restart: always
```

## Contact

If you have any questions in deployment or build and any error is found, please contact me by opening an issue. And contributing is always welcome. The [Github repository url](https://github.com/d1egoprog/stanford-corenlp-docker)

