# Azure Devops Self-Hosted Agents Docker-Compose

Launch self-hosted agents in Azure DevOps using docker-compose. Although a guide has been provided for [self-hosted in Docker](https://learn.microsoft.com/en-us/azure/devops/pipelines/agents/docker?view=azure-devops), there isn't an example available for running using docker-compose. This repository aims to provide an example of how to accomplish this.

## Development

## Prerequisites

1. Have [Docker for desktop](https://www.docker.com/products/docker-desktop) installed on your machine,
2. Have permission to configure agents within your Azure DevOps organisation.

## Usage

1. Clone the repository and navigate to the directory,
2. Copy `.env.example` and create a new `.env` file and assign environment variable values,
3. Build the agent docker image:

   `docker build -t dockeragent:latest .`

4. Start the agents / services:

   `docker-compose -f docker-compose.example.yml up -d`,

5. Stop all agents / services:

   `docker-compose -f docker-compose.example.yml down --remove-orphans`.

Starting the services will launch one agent and one deployment agent and should be available for usage, within their respective pools.

## Environment variables

The following environment variables are used for the services.

| Environment variable | Description                                                                                                                              | Default value          | Required? |
| -------------------- | ---------------------------------------------------------------------------------------------------------------------------------------- | ---------------------- | --------- |
| AZP_URL              | The URL of the Azure DevOps or Azure DevOps Server instance.                                                                             | N/A                    | Yes       |
| AZP_TOKEN            | Personal Access Token (PAT) with Agent Pools (read, manage) scope, created by a user who has permission to configure agents, at AZP_URL. | N/A                    | Yes       |
| AZP_AGENT_NAME       | Agent name.                                                                                                                              | The container hostname | Optional  |
| AZP_POOL             | Agent pool name.                                                                                                                         | Default                | Optional  |
| AZP_WORK             | Work directory                                                                                                                           | `_work`                | No        |
| DEPLOYMENT_AGENT     | Indicate if the agent should be configured as a deployment agent by setting the value as `"true"`                                        | N/A                    | Optional  |
| DEPLOYMENT_POOLNAME  | The deployment pool that the agent should be added to, if `DEPLOYMENT_AGENT` value is `"true"`                                           | N/A                    | Optional  |

Note: `AZP_POOL` does not need to be specified, if the agent is a deployment agent. But instead a valid deployment pool must already be created within the organisation.
