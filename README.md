# Stanford CoreNLP Server - Dockerized Alternative

This repository is dedicated to exposing and tracking a customized Docker Image of the traditional [Stanford CoreNLP server](http://stanfordnlp.github.io/CoreNLP/corenlp-server.html). This build intends to be used as a dedicated service for personal or small research teams.

A ready-to-use [image](https://hub.docker.com/r/d1egoprog/stanford-corenlp) from Docker Hub is provided, with the [deploy instructions](#deploy-alternatives) and the possibility of downloading and customizing the image through the Dockerfile using simple [build instructions](#build-alternatives). The current supported version is 4.5.7; however, it is possible to modify the `VERSION` variable via the Dockerfile.

Also, deployment testing is provided using plain `HTTP` via the `curl` command using the official python library [`stanza`](https://stanfordnlp.github.io/stanza/).

**DISCLAIMER:** this is **not an official documentation** guide. 

## Deploy Alternatives

To deploy the prebuilt docker image, two options are provided: use the `docker` command or the `docker compose` tool from the CLI utility.

### Deploy using Docker CLI

Directly run the `docker` command like the following example. e.g., changing two variables. For more information, check the [Official Documentation](http://stanfordnlp.github.io/CoreNLP/corenlp-server.html)

``` Dockerfile
docker run -e JAVA_XMX=12g -e ANNOTATORS=tokenize,ssplit,parse -p 9000:9000 d1egoprog/stanford-corenlp
```

### Deploy using docker-compose

Download the prepared 'docker-compose.yaml' file from the repository via `wget` and execute the command using the utility.

``` BASH
wget https://raw.githubusercontent.com/d1egoprog/docker-stanford-corenlp/main/compose.yaml
docker compose up -d
```
Happy hacking!! 🖖🖖.

## Testing the Installation

To check the functionality, you can open a web browser window to your docker-engine `IP` and the chosen service, e.g., `PORT=9000`, generally on [localhost:9000](http://localhost:9000). 

### Consuming by HTTP request

Also, to test your local or remote service, just send the following curl request into your preferred CLI.

``` BASH
curl --data 'The quick brown fox jumped over the lazy dog.' 'http://localhost:9000/?properties={%22annotators%22%3A%22tokenize%2Cssplit%2Cpos%22%2C%22outputFormat%22%3A%22json%22}' -o -
```

### Consuming by Library

To use the official python library [`stanza`](https://stanfordnlp.github.io/stanza/) a small example has been prepared in a jupyter notebook, [stanza-example](test/stanza-example.ipynb)

## Build Alternatives

For more information on the configuration and functionality of the [Stanford OpenNLP Server](https://stanfordnlp.github.io/CoreNLP/corenlp-server.html) use the official documentation.

### Build using Docker CLI

To build the image locally, clone the repository: 

``` BASH
git clone https://github.com/d1egoprog/stanford-corenlp-docker.git
```

Use the `docker` command CLI tool to build and run:

``` Dockerfile
docker build -t stanford-corenlp:4.5.7 stanford-corenlp/.
docker run -p 9000:9000 stanford-corenlp:4.5.7
```

All the JVM parameters can be accessed by editing the Dockerfile and rebuilding the image by default; the parameters configured are:

``` Dockerfile
ENV JAVA_XMX 8G
ENV ANNOTATORS all
ENV TIMEOUT_MILLISECONDS 60000
ENV THREADS 5
ENV MAX_CHAR_LENGTH 100000
ENV PORT 9000
```

If you do not want to edit the Dockerfile, environment variables can be overwritten from the `docker run` command, e.g., changing the JVM memory parameter `JAVA_XMX` to reserve more memory. 

``` Docker
docker run -e JAVA_XMX=12g -p 9000:9000 stanford-corenlp:4.5.6
```

### Build using compose

If preferred, a docker compose file is also available with the standard build from the docker file and an override configuration (same parameters from Dockerfile); this should be changed to set your desired annotators specific computing requirements. To run the service, run the command:

``` Docker
docker compose -f build.yaml up -d
```

Or use the compose file to build the Docker image, storing the following into a new `build.yaml` file. Also is possible to override the variables, e.g., changing the JVM memory parameter `JAVA_XMX` to reserve more memory or changing `ANNOTATORS` task performed by the server. 

``` YAML
services:
  stanford_corenlp:
    image: d1egoprog/stanford-corenlp:4.5.6
    ports:
      - "9000:9000"
    environment: 
      - JAVA_XMX=12G
      - ANNOTATORS=tokenize
    restart: always
```

## Contact

If you have any questions in deployment or if any error is found, please contact me or open an issue. And contributing is always welcome. The [Github repository Issues](https://github.com/d1egoprog/docker-stanford-corenlp/issues).
