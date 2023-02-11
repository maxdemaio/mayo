# gittea

We will start with a docker compose file, [docker-compose.yml](./docker-compose.yml).

Since we're using host volumes, we need to configure `USER_UID` and `USER_GID`:

- `USER_UID`: "The UID (Unix user ID) of the user that runs Gitea within the container. Match this to the UID of the owner of the /data volume if using host volumes (this is not necessary with named volumes)."

- `USER_GID`: "The GID (Unix group ID) of the user that runs Gitea within the container. Match this to the GID of the owner of the /data volume if using host volumes (this is not necessary with named volumes)."

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
