# Running AlphaFold2 (from ColabFold) in Azure Machine Learning

<h3 align="right">Colby T. Ford, Ph.D.</h3>


## Docker Image Creation
First, build the custom Azure ML Docker image (with the required dependencies for the MMAE sensitivity model's preprocessing).
```sh
docker build -t alphafold2_aml --build-arg IMAGE_VERSION=$(git rev-parse --short HEAD) .

# docker run --name alphafold2_aml --rm -p 8787:8787 alphafold2_aml
# docker exec -it alphafold2_aml /bin/bash
```

To push the image to DockerHub, run the following:
```sh
docker image tag alphafold2_aml <USERNAME>/alphafold2_aml:latest
docker push <USERNAME>/alphafold2_aml:latest

# docker image tag alphafold2_aml cford38/alphafold2_aml:latest
# docker push cford38/alphafold2_aml:latest
```


To push the image to an Azure Container Registry, run the following:
```sh
az login
az account set --subscription <SUBSCRIPTION_ID>

az acr login --name <CONTAINER_REGISTRY_NAME>

docker tag mmae_aml <CONTAINER_REGISTRY_NAME>.azurecr.io/alphafold2_aml:$(git rev-parse --short HEAD)
docker push <CONTAINER_REGISTRY_NAME>.azurecr.io/alphafold2_aml:$(git rev-parse --short HEAD)
```