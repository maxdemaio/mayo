# gittea

We will start with a docker compose file, [docker-compose.yml](./docker-compose.yml).

When running the container we mount a local folder, `gittea`, on the host machine to the /var/gittea-data directory within the Jenkins container. This is so that the data stored by Jenkins will persist across container restarts. I made this folder like so:

```
cd ~
```

```
mkdir gittea-data
```

We will then run the docker-compose file like so:

```
docker-compose up
```
