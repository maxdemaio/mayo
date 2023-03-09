# Gitea

We create a folder for the hosted docker volume like so:

```
cd ~
```

```
mkdir gitea
```

Next, we add the docker-compose.yaml below to our `gitea` folder too:

```yaml
version: "3"

networks:
  gitea:
    external: false

services:
  server:
    image: gitea/gitea:latest
    container_name: gitea
    environment:
      - USER_UID=1000
      - USER_GID=1000
    restart: always
    networks:
      - gitea
    volumes:
      - ~/gitea:/data
      - /etc/timezone:/etc/timezone:ro
      - /etc/localtime:/etc/localtime:ro
    ports:
      - "3000:3000"
      - "222:22"
```

After, we can run the compose file with the command (-d for detached mode):

```
docker-compose up -d
```

When you set up the initial configuration of gitea, make sure your "ssh domain" matches port 222 like in the compose file. After this is all set, you need to create a SSH key, and add it to gitea. So, you'll have a private key on your local and the public key on gitea. This will authenticate you.
