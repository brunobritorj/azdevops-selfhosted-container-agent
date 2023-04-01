# Quick reference

  - You can use this image to add self-host agents to an Agent Pool in your Azure DevOps organization.
  - Based on [Microsoft :: Run a self-hosted agent in Docker](https://learn.microsoft.com/en-us/azure/devops/pipelines/agents/docker?view=azure-devops#linux)
  - Maintained by: [brunobritorj](https://github.com/brunobritorj)

# Supported tags and respective ```Dockerfile``` links

## Current versions
  - [```latest```, ```ubuntu2004```, ```ubuntu2004-0.3.3```](https://github.com/brunobritorj/azdevops-selfhosted-container-agent/blob/ubuntu2004-0.3.3/ubuntu2004/Dockerfile) (with basic tools)
  - [```ubuntu2004-iac```, ```ubuntu2004-iac-0.3.3```](https://github.com/brunobritorj/azdevops-selfhosted-container-agent/blob/ubuntu2004-iac-0.3.3/ubuntu2004-iac/Dockerfile) (which adds Ansible and Terraform)
## Old versions
  - [```ubuntu2004-0.2.0```](https://github.com/brunobritorj/azdevops-selfhosted-container-agent/blob/v0.2.0/ubuntu2004/Dockerfile)
  - [```ubuntu2004-iac-0.2.0```](https://github.com/brunobritorj/azdevops-selfhosted-container-agent/blob/v0.2.0/ubuntu2004-iac/Dockerfile)
  - [```ubuntu2004-0.1.6```](https://github.com/brunobritorj/azdevops-selfhosted-container-agent/blob/v0.1.6/ubuntu2004/Dockerfile)

# How to use this image

## Variables

- ```AZP_URL```: The Azure DevOps Organization URL
- ```AZP_TOKEN```: Personal Access Token (PAT) with Agent Pools (```read```, ```manage```) scope.
- ```AZP_POOL```: Agent Pool name (default: ```Default```).
- ```AZP_AGENT_NAME```: Agent name (default: Container name).
- ```AZP_WORK```: Work directory (default: ```_work```).

When running, you can supply the ```--once``` argument in order to make the container serve only one job, then it will be terminated.

## Running a self-hosted agent on Docker

If you want to host a single self-host agent in a Docker node, you can use the following command:

```
$ docker run -e AZP_URL={YOUR_ADO_URL} -e AZP_TOKEN={YOUR_PAT} brunobritorj/azdevops-selfhosted
```

## Running self-hosted agents with Helm

Please, refer to [```values.yaml```](helm/values.yaml) in order to check available options.

```
$ helm upgrade --install ado helm/ --set azdevops.url={YOUR_ADO_URL} --set azdevops.token={YOUR_PAT}
```
