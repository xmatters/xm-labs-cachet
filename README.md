# Cachet Status Page
This repo provides installation, configuration and implementation details for integrating xMatters On-Demand with Cachet - an open source status page.

![Disclaimer](https://raw.githubusercontent.com/xmatters/xMatters-Labs/master/media/disclaimer.png?raw=true)

# Pre-Requisites
- xMatters account - If you don't have one, [get one](https://www.xmatters.com/)!
- A Cachet Instance - If you're running on Docker, [check this section out for helpful hints](#Cachet-Status-Page-using-Docker)

# Files
1. [CachetStatusPage.zip](CachetStatusPage.zip?raw=true) - workflow containing all the custom steps for interacting with Cachet through the API

# Installation
## xMatters Setup
1. Login to xMatters, navigate to the Workflow tab and import the Workflow.zip workflow. Details [here](https://help.xmatters.com/ondemand/xmodwelcome/workflows/manage-workflows.htm#ImportExport)

## xMatters Docker Agent
This lab also uses the docker-ized xMatters Agent. This may or may not be necessary depending on your Cachet environment setup. Running both the xMatters Agent & Cachet in containers made the communication between the two much easier, however each step in the workflow can specify whether to run in the cloud or on an agent.

If installing the xMatters Agent using Docker, head over to this repo and follow instructions to configure and run the agent: https://github.com/xmatters/xm-labs-xAgent-Dockerfile

### Running the two together
- It's important to link the two containers together when running with docker. After running `docker-compose up` to start Cachet in the earlier steps be sure to start your xMatters agent with the below added parameters: 

```
docker run --name <XA-AGENT-NAME> -d \
   -e WEBSOCKET_HOST=<XMATTERS-HOST>.xmatters.com \
   -e WEBSOCKET_SECRET=<XMATTERS-KEY> \
   -e OWNER_API_KEY=<OWNER-API-KEY>  \
   -e FRIENDLY_NAME=dockeragent \
   -p 8081 \
   --link <cachet-container-name> \
   --net <cachet-network-name> \
   xa:latest
```

Be sure to replace all of the above info in between the `< >`'s above with data from your environment and configuration.

## xMatters Workflow Configuration
After importing the workflow, browse to it in your xMatters environment and select `Components -> Endpoints`. Edit the Cachet endpoint to point at the root of your Cachet instance:
![Endpoints](media/endpoints.png?raw=true)

![Cachet Endpoint](media/cachet-endpoint.png?raw=true)

If you are runnning Cachet using the docker container above, you can use the IP address of the container and port 8000 for the base root URL of the API with this out of the box setup. You can also add or setup any type of authentication that your Cachet API instance may require on this page.

## Cachet Status Page using Docker
This lab uses Cachet running inside a Docker container mostly for easy use and setup. This workflow can be configured and ran with any Cachet implementation as long as the API can be accessed either from the public internet, or using an xMatters agent.

If installing Cachet using Docker, head over to [this link](https://docs.cachethq.io/docs/get-started-with-docker) and follow the install instructions. Using `docker-compose` makes standing up Cachet quick and easy.

**Note:** You will need to generate an app-key, or copy one from failed output when attempting to run the Cachet container after the first time. The easiest way to do this is look in the terminal output after trying to run the container with `docker-compose up` the first time. See the output below:

![Cachet app-key](media/cachet-app-key.png?raw=true)

Copy the value that's highlighted and paste it in the `docker-compose.yml` file for Cachet. It should follow this format `APP_KEY=base64:<token-value>`. After doing so, run `docker-compose up` again and you should see it successfully start. Browse to localhost and you should see your Cachet status page:

![Cachet Running](media/cachet-running.png?raw=true)

# Testing
Browse to one of the flows in the imported communication plan - recommended to choose "Create Component" to create a test component in Cachet running through xMatters. Select the HTTP trigger step in the workflow and copy the URL. The screen below shows where you can find this URL:

![Create Component Endpoint](media/create-component.png?raw=true)

Take this URL and test POSTing to it using Postman (or your favorite API testing tool). 

The same functionality can be tested from within the xMatters UI as all of these workflows can be initiated from a form as well:
1. Head to the Messaging menu item, and select "Create Component" from the UI menu underneath the Cachet Status Page section 
2. Fill out the form, being sure to fill out all required info.
3. Submit the form using the "Send Message" button.
4. Head over to your Cachet status page and in a few short seconds (or milliseconds) you should see your newly created component.

# Troubleshooting
While testing, you can use the activity stream in the xMatters UI on any of the Flow Designer pages to see a log of what happened. This is highly recommended while setting up the integration for the first time, or while adding or changing any functionality.

![Activity Stream](media/activity-stream.png?raw=true)
