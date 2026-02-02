# Crear un git para un repositorio existente local

## 1	Descripción del procedimiento

Crear un git remoto para un repositorio que tenemos local. 
El objetivo es tener un control de versiones de lo que vamos realizando en nuestro servidor de monitoreo. 
### 1.1	tags

#git
#proyecto
#repositorio

## 2	Requisitos

- Tener acceso a git
- Tener git configurado en nuestra máquina


## 3	Procedimiento

Vamos a crear primero el repositorio remoto. 
Para ello vamos a nuestro git, vamos a "Repositories" y clicamos en "New". Le damos nombre y descripción, en nuestro caso lo hemos llamado monitoring-dockers.
Cambiamos la visibilidad de

Ahora en nuestro servidor nos situamos en el directorio que queremos tener en el git:

```sh
cd /root/monitoring
git init
git add .
git commit -m "Initial commit: add monitoring directory"
git remote add origin git@github.com:genisrosello/monitoring-dockers.git
git branch -M main
git push -u origin main
```


