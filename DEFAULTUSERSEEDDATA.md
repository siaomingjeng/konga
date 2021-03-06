# Changing the default user seed data

Konga will populate the user table with default entries if it finds none are present. If you wish to change the default data you can supply a file with alternative values. A sample file could look like:

````
module.exports = [
        {
            "username": "myadmin",
            "email": "myadmin@some.domain",
            "firstName": "Peter",
            "lastName": "Administrator",
            "node_id": "http://kong:8001",
            "admin": true,
            "active" : true,
            "password": "somepassword"
        },
        {
            "username": "otheruser",
            "email": "otheruser@some.domain",
            "firstName": "Joe",
            "lastName": "Blogs",
            "node_id": "http://kong:8001",
            "admin": false,
            "active" : true,
            "password": "anotherpassword"
        }
    ]
````

To make Konga use this file instead of the hardcoded default users you should set the enviroment variable KONGA_SEED_USER_DATA_SOURCE_FILE to point to the files location:
````
export KONGA_SEED_USER_DATA_SOURCE_FILE=~/userdb.data 
````

This is espically useful when running Konga in a container as part of a Docker swarm. The file can be setup as a Docker secret and supplied to the container. This can be done with an entry in a compose file simular to:

````
version: "3.1"

secrets:
  konga_user_seed:
    external: true

services:
  konga:
    image: pantsel/konga
    secrets:
     - konga_user_seed
    environment:
      - KONGA_SEED_USER_DATA_SOURCE_FILE=/run/secrets/konga_user_seed
    deploy:
      restart_policy:
        condition: on-failure
    ports:
     - 1337:1337
````

(This will work if the swarm is setup with the konga_user_seed secret set with it's value as the contents of the user file.)

