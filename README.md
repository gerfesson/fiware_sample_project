# fiware_sample_project
Repo to create a sample_project using fiware middleware

## Configuring Orion Context Broker with docker

- Run the following commands
```console
export $(cat .env | grep "#" -v)
docker compose -p fiware up -d
```

This will create a container with mongo-db and orion-v2 images

> [!TIP]
> If you got the error "Error response from daemon: Pool overlaps with other one on this address space" try to run the ipconfig command and use the same Hyper-V IPV4 address in docker-compose file on section subnet.

## Checking if the server is running

Now certify that your service is running properly using this command:

```console
curl -X GET \
  'http://localhost:1026/version'
```

## Inserting sample data

Run the following cUrls to insert data into orion service


### Store 1:
```console
curl -iX POST \
  'http://localhost:1026/v2/entities' \
  -H 'Content-Type: application/json' \
  -d '
{
    "id": "urn:ngsi-ld:Store:001",
    "type": "Store",
    "address": {
        "type": "PostalAddress",
        "value": {
            "streetAddress": "Bornholmer Straße 65",
            "addressRegion": "Berlin",
            "addressLocality": "Prenzlauer Berg",
            "postalCode": "10439"
        },
        "metadata": {
            "verified": {
                "value": true,
                "type": "Boolean"
            }
        }
    },
    "location": {
        "type": "geo:json",
        "value": {
             "type": "Point",
             "coordinates": [13.3986, 52.5547]
        }
    },
    "name": {
        "type": "Text",
        "value": "Bösebrücke Einkauf"
    }
}'
```

### Store 2:
```
curl -iX POST \
  'http://localhost:1026/v2/entities' \
  -H 'Content-Type: application/json' \
  -d '
{
    "type": "Store",
    "id": "urn:ngsi-ld:Store:002",
    "address": {
        "type": "PostalAddress",
        "value": {
            "streetAddress": "Friedrichstraße 44",
            "addressRegion": "Berlin",
            "addressLocality": "Kreuzberg",
            "postalCode": "10969"
        },
        "metadata": {
            "verified": {
                "value": true,
                "type": "Boolean"
            }
        }
    },
    "location": {
        "type": "geo:json",
        "value": {
             "type": "Point",
             "coordinates": [13.3903, 52.5075]
        }
    },
    "name": {
        "type": "Text",
        "value": "Checkpoint Markt"
    }
}'
```

## Querying data

To query some data by id, run the following command

```
curl -G -X GET \
   'http://localhost:1026/v2/entities/urn:ngsi-ld:Store:001' \
   -d 'options=keyValues'
```

## References

> https://github.com/FIWARE/tutorials.Getting-Started