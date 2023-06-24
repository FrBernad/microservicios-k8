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