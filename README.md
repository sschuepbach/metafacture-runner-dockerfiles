# Dockerfiles for Metafacture Runner

## About

> Metafacture is a toolkit for processing semi-structured data with a focus on library metadata. It provides a versatile set of tools for reading, writing and transforming data. Metafacture can be used as a stand-alone application or as a Java library in other applications. The name Metafacture is a portmanteau of the words meta data and manufacture.

> Metafacture includes a large number of modules for operating on semi-structured data. These modules can be combined to build pipelines to perform complex metadata processing tasks. The pipelines can be constructed either in Java code or with the domain-specific language Flux. One of the core features of Metafacture is the Metamorph module. Metamorph is an xml-based language for specifying transformations of semi-structured data. It can be seamlessly integrated into Java code.

s. https://github.com/metafacture/metafacture-core

## Versions

- [Metafacture runner 5.0.0](https://github.com/sschuepbach/metafacture-runner-dockerfiles/tree/5.0.0)
- [Metafacture runner 4.0.0](https://github.com/sschuepbach/metafacture-runner-dockerfiles/tree/4.0.0)
- [Metafacture runner 3.1.1](https://github.com/sschuepbach/metafacture-runner-dockerfiles/tree/3.1.1)
- [Metafacture runner 3.1.0](https://github.com/sschuepbach/metafacture-runner-dockerfiles/tree/3.1.0)
- [Metafacture runner 3.0.0](https://github.com/sschuepbach/metafacture-runner-dockerfiles/tree/3.0.0)
- [Metafacture runner 2.0.0](https://github.com/sschuepbach/metafacture-runner-dockerfiles/tree/2.0.0)

## Usage

This repository provides two Dockerfiles to run standalone instances of
Metafacture (so called Metafacture runners). In order to efficiently use
Metafacture, one needs at least provide a Flux file and whose path must be
given as argument to the Docker container. If there is some interaction with
the local filesystem (input or output), the image provides some dedicated
mount points, namely `/in` and `out`. Additional files (Morph definitions,
maps etc.) can be provided via the `/mfwf` mount point. At last you can
integrate Metafacture plugins (like the ones from
[linked-swissbib](https://github.com/linked-swissbib/swissbib-metafacture-commands))
via the internal `/app/plugins` mount point.

The Dockerfile comes in two flavors: The first one for productive use, the
other one for debugging / testing purposes. The latter offers a couple of
additional features:

- A port which can be used by a remote debugger (per default this is `50505`,
  but can be changed by issuing the `--build-arg debugport=<new_port>`
parameter while building the image).
- Issues logs on `DEBUG` level (can be changed via the `--build-arg
  debuglevel=<debuglevel>`). To follow the logs type `docker logs -f
<container_name>` after the container has been started.

To make the two images clearly distinct, it is recommended to build them
with explicit names, e.g.

    docker build -t mfrunner-prod prod

and

    docker build -t mfrunner-debug debug


## Examples

### Start a production-ready container

    docker run \
    -v /my/host/input/dir:/in:ro \
    -v /my/host/output/dir:/out \
    -v /my/morph/dir:/mfwf:ro \
    -v /my/plugins/dir:/app/plugins:ro \
    mfrunner-prod \
    /my/flux/dir \
    flux_var1=value1 flux_var2=value2

### Start a container for a debugging session

    docker run \
    -v /my/host/input/dir:/in:ro \
    -p 50505:50505 \
    mfrunner-prod \
    /my/flux/dir
