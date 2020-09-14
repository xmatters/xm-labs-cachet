# Cachet Status Page

# Pre-Requisites
- xMatters account - If you don't have one, [get one](https://www.xmatters.com/)!

# Files
1. Workflow.zip - workflow containing all the custom steps for interacting with Cachet through the API

# Installation
## xMatters Setup
1. Login to xMatters, navigate to the Workflow tab and import the Workflow.zip workflow. Details [here](https://help.xmatters.com/ondemand/xmodwelcome/workflows/manage-workflows.htm#ImportExport)

## Cachet Status Page using Docker
This lab uses Cachet running inside a Docker container mostly for easy use and setup. This workflow can be configured and ran with any Cachet implementation as long as the API can be accessed either from the public internet, or using an xMatters agent.

If installing Cachet using Docker, head over to [this link](https://docs.cachethq.io/docs/get-started-with-docker) and follow the install instructions. Using `docker-compose` makes standing up Cachet quick and easy.

## xMatters Docker Agent
This lab also uses the docker-ized xMatters Agent. This may or may not be necessary depending on your Cachet environment setup. Running both the xMatters Agent & Cachet in containers made the communication between the two much easier. 

If installing the xMatters Agent using Docker, head over to this repo and follow instructions to configure and run the agent: https://github.com/xmatters/xm-labs-xAgent-Dockerfile

### Running the two together
- It's important to link the two containers together when running with docker. After running `docker-compose up` to start Cachet in the earlier steps be sure to start your xMatters agent with the below added parameters: 

docker run --name <XA-AGENT-NAME> -d \
   -e WEBSOCKET_HOST=acme.xmatters.com \
   -e WEBSOCKET_SECRET=<XMATTERS-KEY> \
   -e OWNER_API_KEY=<OWNER-API-KEY>  \
   -e FRIENDLY_NAME=dockeragent \
   -p 8081 \
   --link cachet-docker_cachet_1 \
   --net cachet-docker_default \
   xa:latest

# Testing


# Troubleshooting

