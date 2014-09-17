# Example Application on CoreOS

## Setup
* Create a CoreOS cluster with `cloud-config.yaml` as userdata e.g. https://www.digitalocean.com/community/tutorials/how-to-set-up-a-coreos-cluster-on-digitalocean
* Upload units

## Ping
* `$ fleetctl start ping.service`
* `$ fleetctl journal -f ping.service`
* `$ curl $(docker inspect --format "{{ .NetworkSettings.IPAddress }}" ping.service)`

## Redis
* `$ fleetctl start registrator.service`
* `$ fleetctl start redis.service`
* `$ docker logs registrator.service`
* `$ etcdctl ls /services/redis`

## Weather Service
* `$ fleetctl start redis-ambassador.service`
* `$ fleetctl journal -f redis-ambassador`
* `$ fleetctl start weather.service`
* `$ fleetctl journal -f weather.service
* `$ curl $(docker inspect --format "{{ .NetworkSettings.IPAddress }}" weather.service):1337`
* `$ docker run --rm -ti dockerfile/redis redis-cli -h $(docker inspect --format "{{ .NetworkSettings.IPAddress }}" redis.service) get currentweather`