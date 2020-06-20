# Docker

## What is a Container?

A container is a standard unit of software that packages up code and all its dependencies so the application runs quickly and reliably from one computing environment to another.

A Docker container image is a lightweight, standalone, executable package of software that includes everything needed to run an application: code, runtime, system tools, system libraries and settings.

## Docker Compose

Compose is a tool for defining and running multi-container Docker applications. With Compose, you use a YAML file to configure your application’s services. Then, with a single command, you create and start all the services from your configuration.

Using Compose is basically a three-step process:

- Define your app’s environment with a Dockerfile so it can be reproduced anywhere.

- Define the services that make up your app in docker-compose.yml so they can be run together in an isolated environment.

- Run docker-compose up and Compose starts and runs your entire app.

## Commands

```shell
docker run --rm -v $(pwd):/app composer install
```

Using the -v and --rm flags with docker run creates an ephemeral container that will be bind-mounted to your current directory before being removed. This will copy the contents of your directory to the container and also ensure that the vendor folder Composer creates inside the container is copied to your current directory.

```shell
docker-compose up -d
```

When you run docker-compose up for the first time, it will download all of the necessary Docker images, which might take a while. Once the images are downloaded and stored in your local machine, Compose will create your containers. The -d flag daemonizes the process, running your containers in the background.

```shell
docker ps
```

List all of the running containers.

```shell
docker ps -a
```

List all containers.

```shell
docker-compose exec app nano .env
```

It allows you to run specific commands in containers. In this case, you are opening the file .env inside the app container for editing.

```shell
docker-compose exec db bash
```

It executes an interactive bash shell on the db container.

```shell
docker rm <container ID> -f
```

Remove one or more containers. The -f option is used to force the removal of a running container.

```shell
docker rm $(docker ps -a -q)
```

This command will delete all stopped containers. The command docker ps -a -q will return all existing container IDs and pass them to the rm command which will delete them. Any running containers will not be deleted.

```shell
docker rmi <image ID> -f
```

Remove one or more images. The -f option is used to force the removal of the image.

```shell
docker rmi $(docker images -q) -f
```

This command will delete all images.

## Dockerfile

Docker can build images automatically by reading the instructions from a Dockerfile.

A Dockerfile is a text document that contains all the commands a user could call on the command line to assemble an image.
