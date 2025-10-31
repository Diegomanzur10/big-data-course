# Guía paso a paso: Crear un clúster Dataproc (Single Node)

1. Cloud Shell

gcloud auth list

gcloud config list project

Corramos esta instruccion en la consola

# grab your “active” project from gcloud config
PROJECT_ID=$(gcloud config get-value project)

# fetch its numeric project number (needed to build the default-SA email)
PROJECT_NUMBER=$(
  gcloud projects describe "${PROJECT_ID}" \
    --format="value(projectNumber)"
)

# build the default-service-account address
SA_EMAIL="${PROJECT_NUMBER}-compute@developer.gserviceaccount.com"

# now bind the role in one command
gcloud projects add-iam-policy-binding "${PROJECT_ID}" \
  --member="serviceAccount:${SA_EMAIL}" \
  --role="roles/dataproc.worker"


2. Habilitar Cloud Resource Manager API

3. Buscamos dataproc en el buscador de gcp, lo habilitamos

Single Node (1 master, 0 workers)

2.1 (Ubuntu 20.04 LTS, Hadoop 3.3, Spark 3.3)
First released on 12/12/2022.

Enable component gateway SI

Jupyter Notebook

e2-standard-4 (4 vCPU, 2 core, 16 GB memory)

Enables the cloud-platform scope for this cluster