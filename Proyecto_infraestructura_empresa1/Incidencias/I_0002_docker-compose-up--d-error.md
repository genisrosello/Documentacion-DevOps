
# Error al ejecutar docker-compose up -d

## 1	Descripción de la incidencia

Error al intentar ejecutar docker-compose up -d por primera vez en el [[P_0003_Instalacion_Grafana+Prometheus_Docker|procedimiento de instalación de dockers grafana y prometheus]]

Cuando intentamos ejecutar el comando aparece el siguiente error:
```sh
docker compose up -d 
unknown shorthand flag: 'd' in -d 
See 'docker --help'.
```

#docker 
#incidencia 
#docker-compose 

## 2	Procedimiento de investigación y resolución

Buscando información en internet vemos que el problema es que docker-compose es un modulo avanzado de docker y hay que instalarlo manualmente.


```sh
sudo apt update
sudo apt install -y docker-compose
```

Ahora ya podemos iniciar los dockers con docker-compose up -d