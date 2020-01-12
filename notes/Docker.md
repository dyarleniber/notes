# Docker

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
