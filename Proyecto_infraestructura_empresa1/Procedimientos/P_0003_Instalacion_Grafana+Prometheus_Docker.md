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

