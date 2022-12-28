# Az DevOps self-hosted container agent

From [Microsoft :: Run a self-hosted agent in Docker](https://learn.microsoft.com/en-us/azure/devops/pipelines/agents/docker?view=azure-devops#linux)

## How to

1. Generate a Personal Access Token

2. Start the image
    ```
    docker run -e AZP_URL=<Azure DevOps instance> -e AZP_TOKEN=<PAT token> ghcr.io/brunobritorj/azdevops-selfhosted-container-agent:main
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
