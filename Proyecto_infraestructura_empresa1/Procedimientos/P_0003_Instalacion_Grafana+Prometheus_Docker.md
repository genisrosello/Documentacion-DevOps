# Instalación de grafana en un docker

## 1	Descripción del procedimiento

Procedimiento para instalar de manera básica y rápida un Grafana y prometheus en un docker compose. Poder levantar este docker y poder consultar desde fuera de la máquina, vía web, las monitorizaciones.

Este entorno será reproducido con imágenes publicas tanto de grafana como de prometheus

### 1.1	tags

#docker
#grafana
#prometheus
#docker-compose 
#monitoring

## 2	Requisitos

- Acceso al servidor Debian
- [[P_0002_Instalacion_Docker|Docker instalado en el servidor]]
- Acceso a internet o a los repositorios oficiales de Docker (para descargar imágenes)


## 3	Procedimiento

Prueba de git de obsidian.

### 3.1	Preparación de entorno

Creamos un directorio que se va a llamar "monitoring" ya que ahí estará todo lo necesario para la monitorización que vamos a hacer a nuestros servidores.  Dentro de monitoring creamos el directorio "prometheus" donde irá el fichero de configuración necesario. Creamos también el fichero de configuración llamado "prometheus.yml"

> [!tip]
> En este caso lo vamos a crear en el directorio root ya que será un servidor pequeño y de poco uso. En un uso real, de un servidor que puede escalar, lo pondríamos en un directorio localizado en un dispositvo de almacenaje diferente al de la raíz del sistema.


```sh
cd /root/
mkdir monitoring
cd monitoring/
mkdir prometheus
cd prometheus/
vim prometheus.yml
```

Dentro del fichero de prometheus vamos a poner la configuración más básica. Más adelante lo editaremos con lo que nos interese.

```yml
global:
  scrape_interval: 15s

scrape_configs:
  - job_name: "prometheus"
    static_configs:
      - targets: ["localhost:9090"]
```


### 3.2	Configuración de Docker

Vamos a crear el fichero de configuración de Docker llamado docker-compose.yml donde configuramos 2 dockers diferentes. Uno para prometheus y otro para Grafana. Lo vamos a hacer en el directorio monitoring

```sh
cd /root/monitoring/
```

Creamos el fichero docker-compose.yml

```sh
vim docker-compose.yml
```

Le pegamos el siguiente contenido

```yml
version: "3.8"

services:
  prometheus:
    image: prom/prometheus:latest
    container_name: prometheus
    restart: unless-stopped
    ports:
      - "9090:9090"
    volumes:
      - ./prometheus/prometheus.yml:/etc/prometheus/prometheus.yml:ro
      - prometheus-data:/prometheus

  grafana:
    image: grafana/grafana:latest
    container_name: grafana
    restart: unless-stopped
    ports:
      - "3000:3000"
    volumes:
      - grafana-data:/var/lib/grafana
    depends_on:
      - prometheus

volumes:
  prometheus-data:
  grafana-data:
```

Como podemos ver son dockers que se descargan las imagenes oficiales de prometheus y grafana del hub de docker oficial.

Explicación de los parámetros:
#todo


## 4	Arrancar dockers

> [!note]
> Si es la primera vez que vas a usar docker-compose, es posible que necesites instalar su dependencia. Revisa la incidencia: [[I_0002_docker-compose-up--d-error]]


Ejecutamos el siguiente comando para iniciar los dockers. La primera vez va a tener que descargar las imágenes del repositorio oficial de docker.

```sh
docker-compose up -d
```

### Verificación de dockers iniciados
