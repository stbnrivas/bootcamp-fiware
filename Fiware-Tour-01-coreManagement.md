# Context


## entities

An entity represents the state of a physical or conceptural object which exists in the real world.

- multitenant support with `Fiware-Service: service` header

### entities crud


- create

```bash
curl -X POST \
  http://localhost:1026/v2/entities?options=keyValues \
  -H 'Content-Type: application/json' \
  -H 'Fiware-Service: service' \
  -d '{
        "id":"urn:ngsi-ld:Vertical:City:entity_type:sha-1",
        "type":"Thing",
        "example_attributte":"example_value"
      }'
```
- read

get all

```bash
curl -X GET \
  'http://localhost:1026/v2/entities?options=keyValues' \
  -H 'Fiware-Service: service'
```

filtering by type

```bash
curl -X GET \
  'http://localhost:1026/v2/entities?type=Thing&options=keyValues' \
  -H 'Fiware-Service: service'
```

filtering by id

```bash
curl -X GET \
'http://localhost:1026/v2/entities/urn:ngsi-ld:Vertical:City:entity_type:sha-1?type=Thing&options=keyValues' \
-H 'Fiware-Service: service'
```

read attributtes

- update

```
curl -X PUT \
  'http://localhost:1026/v2/entities/urn:ngsi-ld:Vertical:City:entity_type:sha-1/attrs?options=keyValues' \
  -H 'Content-Type: application/json' \
  -H 'Fiware-Service: service' \
  -d '{ "example_attributte":"example_value_changed" }'
```

- remove

```
curl -X DELETE \
  http://localhost:1026/v2/entities/urn:ngsi-ld:Vertical:City:entity_type:sha-1 \
  -H 'Fiware-Service: service'
```

### subscriptions to entities changes

to see the request which inform about changes can use https://postb.in or locally if your request needs privacy

```bash
git clone https://gitlab.com/webdev.sh/postb.in
```

using the https://postb.in > create bin > take the url `https://postb.in/b/1575115534177-9746196737978`

```bash
curl -H 'X-Status: Awesome' https://postb.in/1575116751620-8678458128124
```


- create a entity

```bash
curl -X POST \
  http://localhost:1026/v2/entities?options=keyValues \
  -H 'Content-Type: application/json' \
  -H 'Fiware-Service: service' \
  -d '{
        "id":"temperature001",
        "type":"Temperature",
        "measurement":"25"
      }'
```

- get by id

```bash
curl -X GET \
'http://localhost:1026/v2/entities/temperature001?options=keyValues' \
-H 'Fiware-Service: service'
```

- create a subscription (pointing to httpbin)

```bash
curl -X POST \
  'http://localhost:1026/v2/subscriptions' \
  -H 'Content-Type: application/json' \
  -H 'Fiware-Service: service' \
  -d '{
        "description": "a simple subscription for temperature001",
        "subject": {
          "entities": [
            {
              "id": "temperature001",
              "type":"Temperature"
            }
          ],
          "condition": {
            "attrs": [ "measurement" ]
          }
        },
        "notification": {
          "attrs": ["id", "type", "temperature"],
          "http": {
            "url": "https://postb.in/1575116751620-8678458128124"
          },
          "attrsFormat": "legacy"
        },
        "expires": "2020-12-05T09:00:00.00Z"
      }'
```

- get all subscriptions

```bash
curl -X GET \
  'http://localhost:1026/v2/subscriptions' \
  -H 'Fiware-Service: service' | jq
```

- update temperature001 entity

```bash
curl -X PUT \
  'http://localhost:1026/v2/entities/temperature001/attrs?options=keyValues' \
  -H 'Content-Type: application/json' \
  -H 'Fiware-Service: service' \
  -d '{ "measurement":"21" }'
```

- see at https://postb.in/b/1575115534177-9746196737978


### geoqueries

with a few of entities not interesting yet...





# Context Store

Context information stored by the Fiware Context Broker only includes the latest value of entity attributes. Fiware has differents alternatives to do that task.

- STH-Comet (short term historical)

- Cygnus Store Context Information

  preferred because it is part of synchronicity

- Draco Store Context Information
