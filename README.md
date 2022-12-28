# Az DevOps self-hosted container agent

From [Microsoft :: Run a self-hosted agent in Docker](https://learn.microsoft.com/en-us/azure/devops/pipelines/agents/docker?view=azure-devops#linux)

## How to

1. Generate a Personal Access Token

2. Run the container using the proper image
    ```
    docker run -e AZP_URL={AzDevOps Org url} -e AZP_TOKEN={PAT} ghcr.io/brunobritorj/azdevops-selfhosted/ubuntu2004
    ```

3. Check the new registed agent at AzDevOps portal, ```{Organization} / Settings / Agent pools / {Pool} / Agents```

## Variables

Then only two mandatory vars are ```AZP_URL``` and ```AZP_TOKEN```, but you can use other vars to customize your agent:

|Env variable|Description|
|-|-|
|AZP_URL|The URL of the Azure DevOps or Azure DevOps Server instance.|
|AZP_TOKEN|Personal Access Token (PAT) with Agent Pools (read, manage) scope, created by a user who has permission to configure agents, at AZP_URL.|
|AZP_AGENT_NAME|Agent name (default value: the container hostname).|
|AZP_POOL|Agent pool name (default value: Default).|
|AZP_WORK|Work directory (default value: _work).|

We can also specify the ```--once``` arg in order to the agent accept only one job.

## Kubernetes

```
kubectl create secret generic azdevops \
  --from-literal=AZP_URL={AzDevOps Org url} \
  --from-literal=AZP_TOKEN={PAT} \
  --from-literal=AZP_POOL={Pool}
```
```yaml
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
        image: ghcr.io/brunobritorj/azdevops-selfhosted/ubuntu2004:latest
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