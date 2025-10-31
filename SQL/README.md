# Guía paso a paso: VM en GCP para Jupyter / PySpark

Esta guía resume cómo crear una instancia de máquina virtual (VM) en Google Cloud Platform, instalar Docker y levantar notebooks Jupyter (incluyendo PySpark) de forma segura.

---
## 1. Crear la instancia de VM

1. Ir a Compute Engine > VM instances > Create Instance.
2. Definir el "Machine type": por ejemplo `e2-standard-4` o uno con ~16 GB RAM (ajusta según tu presupuesto).
3. Cambiar el Sistema Operativo a **Ubuntu 20.04 LTS**.
4. Aumentar el disco: **100 GB** (SSD recomendado si vas a trabajar con más I/O).
5. Marcar las opciones de Firewall: Allow HTTP y Allow HTTPS si planeas exponer servicios web (opcional para Jupyter puro).

> Tip: Etiqueta la instancia (labels) para facilitar reglas de firewall específicas si manejas varias VMs.

---
## 2. Configurar Firewall (puertos Jupyter / Dash)

Necesitamos abrir los puertos:

- `8888` para Jupyter Notebook / Lab
- `8050` para aplicaciones Dash (opcional)

Pasos:
1. Ir a VPC Network > Firewall.
2. Crear regla de firewall nueva:
    - Target: la instancia o tag usado.
    - Protocols & ports: `tcp:8888,8050`
    - Source IP ranges: `0.0.0.0/0` (solo para pruebas; en producción limita tu IP pública).

---
## 3. Conectarse y validar el disco

Desde la consola SSH de la VM ejecuta:

```bash
df -h
```

Verifica que el disco asignado (100 GB) aparece correctamente.

---
## 4. Instalar Docker

Ejecuta los siguientes comandos en la VM (Ubuntu 20.04):

```bash
sudo apt-get update
sudo apt-get install -y \
   ca-certificates \
   curl \
   gnupg \
   lsb-release

sudo mkdir -p /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg

echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | \
   sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt-get update
sudo apt-get install -y docker-ce docker-ce-cli containerd.io docker-compose-plugin

sudo docker run hello-world
```

Si ves el mensaje de bienvenida de Docker, la instalación fue exitosa.

---
## 5. Ejecutar contenedores Jupyter

### Opción A: SciPy / Data Science base
```bash
docker run -d --name jupyter-scipy -p 8888:8888 \
   -e JUPYTER_TOKEN="CAMBIA_ESTE_TOKEN" \
   jupyter/scipy-notebook
```

### Opción B: Notebook con PySpark
```bash
docker run -d --name jupyter-pyspark -p 8888:8888 \
   -e JUPYTER_TOKEN="CAMBIA_ESTE_TOKEN" \
   jupyter/pyspark-notebook
```

Luego abre en tu navegador:
```
http://IP_PUBLICA_DE_LA_VM:8888/?token=CAMBIA_ESTE_TOKEN
```

> Seguridad: Evita usar contraseñas simples como `MY_EASY_PASSWORD`. Usa tokens largos y considera restringir el acceso por IP en la regla de firewall.
