# Guía: Crear un clúster Dataproc (Single Node)

Este documento describe cómo preparar permisos y crear un clúster Dataproc de un solo nodo (1 master, 0 workers) con Jupyter habilitado.

## 1. Preparar entorno en Cloud Shell

Verifica cuenta y proyecto activo:
```bash
gcloud auth list
gcloud config list project
```

Exporta variables y asigna el rol necesario al Service Account por defecto:
```bash
# Proyecto activo
PROJECT_ID="$(gcloud config get-value project)"

# Número de proyecto
PROJECT_NUMBER="$(gcloud projects describe "${PROJECT_ID}" --format="value(projectNumber)")"

# Service Account de Compute Engine
SA_EMAIL="${PROJECT_NUMBER}-compute@developer.gserviceaccount.com"

# Conceder rol de trabajador de Dataproc
gcloud projects add-iam-policy-binding "${PROJECT_ID}" \
  --member="serviceAccount:${SA_EMAIL}" \
  --role="roles/dataproc.worker"
```

## 2. Habilitar APIs necesarias

```bash
gcloud services enable \
  compute.googleapis.com \
  dataproc.googleapis.com \
  cloudresourcemanager.googleapis.com
```

## 3. Crear clúster Single Node

En consola web (Dataproc > Create Cluster) o vía CLI.

Parámetros recomendados:
- Nombre: ejemplo `spark-single-node`
- Región: selecciona una cercana (ej. `us-central1`)
- Tipo de clúster: Single Node (1 master, 0 workers)
- Imagen: Ubuntu 24.04 LTS (Hadoop 3.3 / Spark 3.3)
- Máquina master: `e2-standard-4` (4 vCPU / 16 GB RAM)
- Component Gateway: habilitado (para acceder a UIs)
- Jupyter: marcar componente Jupyter
- Scope adicional: `cloud-platform` (para acceso amplio a otros servicios si se requiere)

CLI equivalente:
```bash
gcloud dataproc clusters create spark-single-node \
  --region=us-central1 \
  --single-node \
  --image-version=2.1-ubuntu20 \
  --properties spark:spark.sql.shuffle.partitions=8 \
  --enable-component-gateway \
  --optional-components=JUPYTER \
  --master-machine-type=e2-standard-4 \
  --master-boot-disk-size=100GB \
  --scopes=cloud-platform
```

## 4. Acceso a Jupyter

Una vez el estado sea RUNNING:
1. Ir a Dataproc > Clusters > spark-single-node.
2. Abrir la pestaña Web Interfaces (Component Gateway).
3. Entrar a Jupyter y crear notebook PySpark (kernel Spark preconfigurado).

## 5. Limpieza (para evitar costos)

```bash
gcloud dataproc clusters delete spark-single-node --region=us-central1
```

## 6. Notas

- Ajusta `spark.sql.shuffle.partitions` según tamaño de datos (valor por defecto suele ser 200).
- Para más memoria: usar `e2-standard-8` o familias N2 si necesitas más rendimiento.
- Para multi-nodo: reemplaza `--single-node` por `--num-workers=<N>` y añade parámetros de tipo de workers.

Fin.