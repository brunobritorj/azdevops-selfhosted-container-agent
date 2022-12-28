# Quick reference

  - You can use this image to add self-host agents to an Agent Pool in your Azure DevOps organization.
  - Based on [Microsoft :: Run a self-hosted agent in Docker](https://learn.microsoft.com/en-us/azure/devops/pipelines/agents/docker?view=azure-devops#linux)
  - Maintained by: [brunobritorj](https://github.com/brunobritorj)

# Supported tags and respective ```Dockerfile``` links

## Current versions
  - [```ubuntu2004-0.2.0```, ```ubuntu2004```, ```latest```](https://github.com/brunobritorj/azdevops-selfhosted-container-agent/blob/v0.2.0/ubuntu2004/Dockerfile)
  - [```ubuntu2004-iac-0.2.0```, ```ubuntu2004-iac```](https://github.com/brunobritorj/azdevops-selfhosted-container-agent/blob/v0.2.0/ubuntu2004-iac/Dockerfile)
## Old versions
  - [```ubuntu2004-0.1.6```](https://github.com/brunobritorj/azdevops-selfhosted-container-agent/blob/v0.1.6/ubuntu2004/Dockerfile)

# How to use this image

## Variables

- ```AZP_URL```: The Azure DevOps Organization URL
- ```AZP_TOKEN```: Personal Access Token (PAT) with Agent Pools (```read```, ```manage```) scope.
- ```AZP_POOL```: Agent Pool name (default: ```Default```).
- ```AZP_AGENT_NAME```: Agent name (default: Container hostname).
- ```AZP_WORK```: Work directory (default: ```_work```).

When running, you can supply the ```--once``` argument in order to make the container serve only one job, then it will be terminated.

## Running a self-hosted agent on Docker

If you want to host a single self-host agent in a Docker node, you can use the following command:

```
$ docker run -e AZP_URL={AzDevOps Org url} -e AZP_TOKEN={PAT} brunobritorj/azdevops-selfhosted:ubuntu2004
```

## Running self-hosted agents on Kubernetes

If you want to host self-hosted agents in a Kubernetes cluster, you can use this deployment file as reference:

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: azdevops-agent
  labels:
    app: azdevops-agent
spec:
  replicas: 1
  selector:
    matchLabels:
      app: azdevops-agent
  template:
    metadata:
      labels:
        app: azdevops-agent
    spec:
      containers:
      - name: azdevops-agent
        image: brunobritorj/azdevops-selfhosted:ubuntu2004
        args: ["--once"]
        env:
          - name: AZP_URL
            valueFrom:
              secretKeyRef:
                name: azdevops
                key: AZP_URL
          - name: AZP_TOKEN
            valueFrom:
              secretKeyRef:
                name: azdevops
                key: AZP_TOKEN
          - name: AZP_POOL
            valueFrom:
              secretKeyRef:
                name: azdevops
                key: AZP_POOL
```

Note that this deployment specification requires 3 secrets with your Az DevOps organization values which you can create with the following command:

```
$ kubectl create secret generic azdevops \
  --from-literal=AZP_URL={AzDevOps Org url} \
  --from-literal=AZP_TOKEN={PAT} \
  --from-literal=AZP_POOL={Pool}
```
