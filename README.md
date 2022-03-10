# Prerequirements:
[Azure Functions Core Tools](https://github.com/Azure/azure-functions-core-tools)
[Docker](https://docs.docker.com/get-docker/)
An Azure Container Registry
Azure DevOps connection to your Azure container registry

# Function
Initiate the function and test it.

```console
func init --worker-runtime node --language typescript --docker

func new --name HttpExample --template "HTTP trigger" --authlevel anonymous

npm install

npm start
```

# Docker
Edit the dockerfile to use the newest [mcr.microsoft.com/azure-functions](https://hub.docker.com/_/microsoft-azure-functions-node) file.

```bash
FROM mcr.microsoft.com/azure-functions/node:4-node16
```

Test it:

```console
docker build --tag appname:v1.0.0 .

docker run -p 8080:80 -it appname:v1.0.0
```

[http://localhost:8080/api/HttpExample?name=jacob](http://localhost:8080/api/HttpExample?name=jacob)


# Azure Portal

1. Create a Resource Group

![](figs/01-resourcegroup.PNG)

2. Create functions app

![](figs/02-functionapp1.PNG)

![](figs/02-functionapp2.PNG)

3. Add deploymentslot for test and setup credentials from the Azure Container Registry

![](03-functionapp-setup1.PNG)

![](03-functionapp-setup2.PNG)


# Azure DevOps

1. Setup the pipeline

![](figs/04-pipelinesetup1.PNG)

![](figs/04-pipelinesetup2.PNG)

![](figs/04-pipelinesetup3.PNG)

# Don't Save!

If you use a different account for azure devops that doesn't have access to your azure subscriptions you can't use the docker for container registries, so yeah...

Edit the yaml file to be like:

```yaml
trigger:
  - main

jobs:
  - job: BuildDockerImage
    displayName: 'Build Docker Image'
    pool:
      vmImage: 'ubuntu-latest'
    steps:
      - task: Docker@2
        inputs:
          containerRegistry: 'name of your azure container registry' # Your docker registry service connection
          repository: 'Repository Name' # Your docker repository name
          command: 'buildAndPush'
          Dockerfile: '**/Dockerfile'
          addPipelineData: false
          addBaseImageData: false
```

2. Setup the release plan

![](figs/05-releasesetup1.PNG)

![](figs/05-releasesetup2.PNG)

![](figs/05-releasesetup3.PNG)

![](figs/05-releasesetup4.PNG)

![](figs/05-releasesetup5.PNG)

![](figs/05-releasesetup6.PNG)

![](figs/05-releasesetup7.PNG)

![](figs/05-releasesetup8.PNG)

![](figs/05-releasesetup9.PNG)

# DONT FORGET TO NAME YOUR RELEASE SETUP LIKE I DID!
