# Akeneo and Apache on Docker

This is a Docker development environment for Akeneo PIM with Apache and `mod_php`.

This environment is based on [carcel/apache-php](https://hub.docker.com/r/carcel/apache-php/).

## How to use it?

### From Docker hub

You can directly pull this image from [Docker hub](https://hub.docker.com/r/carcel/akeneo-apache/) by running:

```bash
$ docker run --name akeneo -p 8080:80 -d carcel/akeneo-apache
```

Access the URL `localhost:8080` with your web browser to check that the container works.

### From GitHub

Clone the repository, go inside the created folder, and build the docker image:

```bash
$ docker build -t "akeneo-apache" .
```

Then you can run a container like this:

```bash
$ docker run --name akeneo -p 8080:80 -d akeneo-apache
```

### Use this image with Docker Compose

The easiest way to use these images is [Docker Compose](https://docs.docker.com/compose/). You can found detailed explanations [here](https://github.com/damien-carcel/Dockerfiles/blob/master/Docs/compose.md).

## License

This repository is under the MIT license. See the complete license in the [LICENSE](https://github.com/damien-carcel/Dockerfiles/blob/master/LICENSE) file.
