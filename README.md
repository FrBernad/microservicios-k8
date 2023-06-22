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

FIXME: subir imagenes a un repo para poder descargarlas en helm. Crear helm chart


docker build -t airports -f ./trips/trips-health/airports/Dockerfile ./trips/trips-health/
docker build -t trips -f ./trips/trips-health/trips/Dockerfile ./trips/trips-health/
docker build -t gateway -f ./trips/trips-health/gateway/Dockerfile ./trips/trips-health/

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