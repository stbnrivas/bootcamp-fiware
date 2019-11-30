# First Edition Bootcamp Fiware - December 2019


# getting started

- make sure to have installed

 + curl
 + jq
 + docker
 + python ( docker-compose pre-requirement)
 + docker-compose

- get docker compose file

```curl
curl https://raw.githubusercontent.com/telefonicaid/fiware-orion/master/docker/docker-compose.yml -o docker-compose.yml
```
it looks like

```yaml
# Note that mongo container is started before Orion, as Orion needs it
# as dependency and reserve the order has been found problematic in some
# low resource hosts
mongo:
  image: mongo:3.6
  command: --nojournal

orion:
  image: fiware/orion
  links:
    - mongo
  ports:
    - "1026:1026"
  command: -dbhost mongo
```

- some minor modifications

```yaml
# Note that mongo container is started before Orion, as Orion needs it
# as dependency and reserve the order has been found problematic in some
# low resource hosts
mongo:
  image: mongo:3.6
  command: --nojournal

orion:
  image: fiware/orion:2.3.0
  links:
    - mongo
  ports:
    - "1026:1026"
  command: -dbhost mongo
```


- run the container with

```bash
# cd bootcamp-fiware
docker-compose up
```

- your first request

```bash

curl -X GET localhost:1026/version -H 'Accept: application/json' | jq
```



- take a look documentation of FIWARE-NGSI v2 Specification

  + [https://swagger.lab.fiware.org/?url=https://raw.githubusercontent.com/Fiware/specifications/master/OpenAPI/ngsiv2/ngsiv2-openapi.json](https://swagger.lab.fiware.org/?url=https://raw.githubusercontent.com/Fiware/specifications/master/OpenAPI/ngsiv2/ngsiv2-openapi.json)

  + [http://fiware.github.io/specifications/ngsiv2/stable/](http://fiware.github.io/specifications/ngsiv2/stable/)


# researching into containers

```bash
docker ps
docker attach 661e4a617f8c
docker exec -it 661e4a617f8c /bin/bash

# id
# id -u
# id -g
docker exec -it --user root 661e4a617f8c /bin/bash
docker exec -it --user 0 661e4a617f8c /bin/bash
```
