# Autores

- [Francisco Bernad](https://github.com/FrBernad)
- [Agustín Manfredi](https://github.com/imanfredi)
- [Ignacio Vazquez](https://github.com/igvazquez)
- [Joaquín Leggamare](https://github.com/jleggamare)

# TP Kubernetes

## Consigna

Se debe desarrollar un Chart de Helm para desplegar en Kubernetes el proyecto de trips. 
El despliegue final debe cumplir con la siguientes características:

- Se debe desplegar esta versión: https://github.com/itba-fcastaneda/73.40-Arquitectura-de-Microservicios/tree/main/clase-09/trips-health
- Debe incluir redundancia de pods para los servicios basados en Python.
- Todos los archivos de configuración deben ser definidos usando ConfigMap o Secrets.
- Se debe elegir el mejor método de despliegue de los pods para cada uno de los servicios.
- Exponer la API rest por medio de un ClusterPort.
- El servicio de Redis debe disponer de persistencia. Pueden usar un HostPath o local con las consideraciones de affinity correspondientes.

FIXME: 
- Subir imagenes a un repo para poder descargarlas en helm. 
- Crear helm chart.
- Ver healtheck con mq.
- Probar en otra compu q a agus no le levanta
https://github.com/docker-library/rabbitmq/pull/174#issuecomment-452002696
https://devops.stackexchange.com/questions/12092/docker-compose-healthcheck-for-rabbitmq


docker build -t airports -f ./trips/trips-health/airports/Dockerfile ./trips/trips-health/airports
docker build -t trips -f ./trips/trips-health/trips/Dockerfile ./trips/trips-health/trips
docker build -t gateway -f ./trips/trips-health/gateway/Dockerfile ./trips/trips-health/gateway

kind create cluster --config kind-config/cluster-config.yaml --name ms 

kind load docker-image airports --name ms
kind load docker-image trips --name ms
kind load docker-image gateway --name ms


AIRPORTS
curl --header "Content-Type: application/json" \
  --request POST \
  --data '{"airport": "Ezeiza"}' \
  172.31.0.3:30001/airport

curl --request GET \
  172.31.0.3:30001/airport/ce822be9bebf4c9bbe48f5591087a047

TRIPS

curl --header "Content-Type: application/json" \
  --request POST \
  --data '{"airport_from":"EZE","airport_to":"CPC"}' \
  172.31.0.3:30001/trip

El pedido devolverá el ID asignado al viaje. Este ID podrá ser utilizado en consultas posteriores como:

curl 172.31.0.3:30001/trip/<TRIP_ID>

helm install trips-app ./k8
helm uninstall trips-app

Build de imágenes

https://docs.docker.com/build/building/multi-platform/
docker buildx ls            
docker buildx create --name multi-os-driver --driver docker-container --bootstrap --use

docker buildx build --platform linux/amd64,linux/arm64 -t docker.io/quickcart/airports:latest -f ./trips/airports/Dockerfile ./trips/airports --push 
docker buildx build --platform linux/amd64,linux/arm64 -t "docker.io/quickcart/trips:latest" -f ./trips/trips/Dockerfile ./trips/trips --push 
docker buildx build --platform linux/amd64,linux/arm64 -t "docker.io/quickcart/gateway:latest" -f ./trips/gateway/Dockerfile ./trips/gateway --push 

docker image tag "airports" "airports:latest"
docker image tag "trips" "trips:latest"
docker image tag "gateway" "gateway:latest"

docker image tag "airports" "docker.io/quickcart/airports:latest"
docker image tag "airports" "docker.io/quickcart/airports"
docker image tag "trips" "docker.io/quickcart/trips:latest"
docker image tag "trips" "docker.io/quickcart/trips"
docker image tag "gateway" "docker.io/quickcart/gateway:latest"
docker image tag "gateway" "docker.io/quickcart/gateway"