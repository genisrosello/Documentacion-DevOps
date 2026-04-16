# Instalación de Ollama, Agente IA en nuestro Debian.

## 1	Descripción del procedimiento

Instalación de un asistente de IA autónomo en nuestro servidor, sin tener que depender de IAs que se conectan a internet o requieren suscripciones de pago.

### 1.1	tags

#IA
#Ollama
#Debian
## 2	Requisitos

- Acceso a internet
- Permisos de administrador
- S.O Debian

## 3	Procedimiento


Con permisos de administrador:
```bash
curl -fsSL https://ollama.com/install.sh | sh
```


En nuestro caso ha tardado 7 minutos en instalar todo lo necesario.

Es posible que te pida actualizar drivers de Nvidia, si lo haces es posible que se rompa el entorno gráfico de Debian.

Requiere reinicio

```bash
reboot
```

hay que hacer ollama run llama3 y ya se puede "hablar con la IA"

```bash
ollama run llama3
```


