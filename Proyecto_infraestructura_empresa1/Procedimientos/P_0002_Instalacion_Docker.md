# Instalación de software Docker


## 1	Descripción del procedimiento

Procedimiento a seguir para la instalación de Docker en un servidor Debian

### 1.1	tags

#Docker
#instalacion
#configuracion

## 2	Requisitos

- Acceso al repositorio de docker 


## 3	Procedimiento


Primero actualizamos los repositorios
```bash
apt update
apt upgrade -y
```

Añadimos la clave GPG de docker

```bash
mkdir -p /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/debian/gpg | gpg --dearmor -o /etc/apt/keyrings/docker.gpg
```

En nuestro caso añadimos el repositorio oficial de Docker:
```bash
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] \
  https://download.docker.com/linux/debian $(lsb_release -cs) stable" | \ 
  tee /etc/apt/sources.list.d/docker.list > /dev/null
```

Instalación de Docker:
```sh
sudo apt update
sudo apt install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

Comprobación de funcionamiento

```sh
sudo docker --version
sudo docker run hello-world
```


## 4	Alternativa

Si no queremos realizar tantos pasos y no importa que la versión de Docker no sea la última, sino la que mantiene Debian como versión estable podemos instalarlo usando:

```sh
sudo apt install docker.io
```