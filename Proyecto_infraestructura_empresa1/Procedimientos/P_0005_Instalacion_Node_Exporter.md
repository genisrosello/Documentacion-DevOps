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

Con esto ya tenemos un docker llamado node-exporter en el puerto 9100 de nuestro servidor.

### 3.2	Enlace con Prometheus

Ahora hay que configurar el fichero de prometheus para que use las métricas de Node Exporter.
Para ello editaremos el fichero de configuración de Prometheus. En nuestro caso tenemos ese fichero en el directorio /root/monitoring/prometheus/

Primero hacemos un backup y luego editamos el fichero

```sh
cd /root/monitoring/prometheus/
cp prometheus.yml prometheus.yml.20260202
vim prometheus.yml
```

Vamos a añadirle el siguiente contenido:

```yml
  - job_name: 'node-exporter'
    static_configs:
      - targets: ['node-exporter:9100']
```

Es importante que el target sea el nombre del servicio y no localhost.

El fichero resultante será:

```yml
global:
  scrape_interval: 15s

scrape_configs:
  - job_name: "prometheus"
    static_configs:
      - targets: ["localhost:9090"]

  - job_name: 'node-exporter'
    static_configs:
      - targets: ['node-exporter:9100']

```

### 3.3	Reinicio de servicios

Para que aplique los cambios debemos reiniciar Prometheus, aprovechamos y arrancamos por primera vez Node Export:

```sh
cd /root/monitoring/
docker-compose up -d node-export
docker-compose restart prometheus
```

#### 3.3.1	Verificación de funcionamiento

Primero comprobamos que Node Export esta funcionando conectándonos a nuestra IP:9100. En mi caso http://192.168.0.50:9100/

Ahora verificamos que podemos enlazar Prometheus con Node Exporter. Vamos a nuestra IP:9090 (portal de prometheus) y vamos a targets. En mi caso http://192.168.0.50:9090/targets
Allí debe aparecer ahora el target node-exporter (es lo que hemos configurado en [[#3.2 Enlace con Prometheus]])

### 3.4	Dashboard de recursos en Grafana

Pondremos los Dashboards Node Exporter Full.
Vamos al portal de Grafana conectándonos a nuestra IP:3000. En mi caso http://192.168.0.50:3000/
Vamos al apartado de dashboard y desplegamos "New". Seleccionamos "import".
Hemos encontrado los dashboards  Node Exporter Full y tienen el ID 1860, lo insertamos y clicamos en "Load"

Ya debería aparecer los dashboards importados con los recursos del server.
