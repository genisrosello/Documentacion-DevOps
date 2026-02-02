# Instalación de Node Exporter


## 1	Descripción del procedimiento

Procedimiento para realizar la instalación de Node Exporter con Docker en servidor Debian. El objetivo de este Node Exporter es enlazarlo con Prometheus que a su vez está conectado al Grafana, así podremos ver en tiempo real los recursos utilizados de nuestro server.
### 1.1	tags


#monitorizacion
#NodeExporter
#docker 


## 2	Requisitos

- Docker instalado y funcionando
- Grafana y Prometheus instalados y funcionando ([[P_0003_Instalacion_Grafana+Prometheus_Docker]] y [[P_0004_Enlace_Grafana_Prometheus]])
- Acceso a internet para descargar imagen Docker oficial


## 3	Procedimiento

### 3.1	Configuración de docker-compose.yml
Cómo la el objetivo de nuestra máquina es tener un docker compose dedicado a monitorizacion vamos a añadirle un docker mas que sea Node Export con la versión mas actual.

Para ello primero haremos un backup del fichero docker-compose.yml y lo editaremos:

```sh
cp docker-compose.yml docker-compose.yml.20260202
vim docker-compose.yml
```

Añadiremos el siguiente contenido:

```yml
  node-exporter:
    image: prom/node-exporter:latest
    container_name: node-exporter
    restart: unless-stopped
    ports:
      - "9100:9100"
    pid: host
    volumes:
      - /:/host:ro,rslave
    command:
      - '--path.rootfs=/host'

```

Quedando un fichero resultante:

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

  node-exporter:
    image: prom/node-exporter:latest
    container_name: node-exporter
    restart: unless-stopped
    ports:
      - "9100:9100"
    pid: host
    volumes:
      - /:/host:ro,rslave
    command:
      - '--path.rootfs=/host'

volumes:
  prometheus-data:
  grafana-data:

```

Con esto ya tenemos un docker llama