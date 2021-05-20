---
layout: post
title: Dockerized Python Project Template 
categories: [Python, Docker, MLOps, Production, Productivity, Template]
---
# Start Here

Before I go into some of the details on Dockerizing a python project for fun and profit, be sure to check out [this](https://eugeneyan.com/writing/setting-up-python-project-for-automation-and-collaboration/) great post on a template python project setup from Euguene Yan. In my opinion it's the best template I've seen that makes it easy to get up and running on a python project while maintaining best practices that not implemented if not done beforehand. These include:

* Python virtual env management

* A `src` code python module setup

* Basic unit tests

* Code coverage

* Type checking

* Code linting

* Make automation for all of the above

* Pre-commit hooks with Github Actions

# Why Docker

Using Docker images have become industry standard for many software  

## Docker Dev Env + Image

Docker is often used for environment management and deployment of production code.

This repo is setup to package things in a Docker image for this purpose.

Through the use of [Docker Compose](https://docs.docker.com/compose/) a dev environment can also be stood up and torn down quickly. Docker compose allows for better environment setup through connected services (e.g. databases, etc) for closer replication of a production environment.

The docker compose file `docker/docker-compose.yml` builds an image from `docker/Dockerfile` and runs a bash shell. 

Environment variables can be added in the relevant section of the `docker-compose.yml` if they are provided in a `.env` file within the `docker` directory. By default the `.env` file is excluded from the repo since it may contain secrets. Instead the file `docker/template.env` is provided which should provide non secret environment variables and the variable name for required secrets.

### Dev Env

To create a dev environment run:

```bash
make dev-env
```

This should create a running docker container with everything required for development in this repo.

All other Make commands should still work as before.

All changes made to relevant files inside the container will be reflected outside the container as they are bound in the `volumes` section of the `docker-compose.yml` file. Any newly added directories or files will need to be added to the `docker/Dockerfile` with a `COPY` command and bound as a volume in the docker compose file.

### Production Image 

Once development is finished and the project is ready to be deployed it can be built and tagged as a Docker image with:

```bash
make build-image
```

The image name and tag are set in the Makefile variables `IMAGE_NAME` and `IMAGE_TAG`.

### Pushing to a container registry

If the name of the image is a container registry, the image can be pushed to the registry with:

```bash
make push-image 
```
