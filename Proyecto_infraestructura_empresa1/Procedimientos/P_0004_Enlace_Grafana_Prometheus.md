# Enlazar Grafana con Prometheus

## 1	Descripción del procedimiento

Configuración del enlace del monitor Grafana con el recolector de datos Prometheus
### 1.1	tags

#grafana 
#prometheus 

## 2	Requisitos

- Tener Grafana y prometheus instalados
- Tener acceso a Grafana

## 3	Procedimiento


Nos conectamos a grafana en nuestro caso con http://192.168.0.50:3000/login

1. Desplegamos el apartado "Connections" y vamos a "Data sources".
2. Añadimos como Data source "Prometheus"


