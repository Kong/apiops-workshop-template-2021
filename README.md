# gitops-demo for use with local Kong environment 

This repository provides a template for doing your own demonstration of the end to end lifecycle. The demonstration uses inso-cli and deck to configure Kong and the Developer portal.

Prerequisites
---

* These instructions assume you already have a a running instance of Kong EE with the Developer Portal enabled in the default workspace. We are assuming a Docker install. 
* This repo has been tested with the [se-tools/demo repo] (https://github.com/Kong/se-tools)
* These instructions assume you have the mocking plugin installed (this is already available in the se-tools/demo-environment repo) [kong-plugin-mocking](https://github.com/Kong/kong-plugin-mocking) repository to your machine.  

* Install [kong-portal-cli](https://github.com/Kong/kong-portal-cli)
* Install [decK](https://docs.konghq.com/hub/kong-inc/deck/)
* Install [inso](https://github.com/Kong/insomnia/tree/develop/packages/insomnia-inso)

Steps for Creating a Running Demo
---
1. Create your own repository with this [template](https://github.com/mannykhadilkar/gitops-demonstration)
1. Clone your repository
1. Use the features picker to select "ENABLE_CUSTOM_PLUGINS" and "ENABLE_REBUILD_IMAGE" to be sure Kong starts with the right configuration for this demonstration. `. ./featuresPicker.sh` 
1. Start Kong according to se-tools/demo-environment instructionss `./kong.sh start docker-compose`  
1. Modify workspace (`/specs/workspace.yaml`) and Actions files (`actions_push.yaml`) to use the workspace you want the dev portal and kong configuration published to (Replace `ProductX` on lines 25 and 28 in `actions_push.yaml` with the workspace name where you would like your changes pushed to)
1. Setup a local Github Actions Runner using Steps from the Github actions workshop.  
1. Once github Actions is online you can begin the demo.  
1. Make sure you have the client id and secret for the okta IdP's client credentials if you are showing openid plugin.  
1. Remove the openid connect plugin and the rate-limiting plugin from the `/specs/orders.yaml` spec if you want to show iteration of how you can add plugins. 

Demonstration Steps
---

## Gitops End to End Flow

**Tell**

1. Kong helps you deploy services faster by enabling automation and decentralization of common logic.  
1. We can use an OAS spec to configure the service registry in Kong and to publish documentation to the API catalog.  

**Show**
1. In Insomnia send a request to `curl --request GET \
  --url http://localhost:8000/v1/order/1 ' to show the service does not exist. 
1. Show Insomonia or vscode with the OAS Pizza order spec that defines the contract.
1. Make a change to the orders.yaml spec directly on the Github repo and create a PR. Alternatively you can create a loal branch and push the changes to Git using vscode or git CLI. 
1. Go to Github to view the PR that gets created, then merge the PR. 
1. Show the actions that get executed. inso generates a deck yaml from an oas spec. inso is also capable of running tests against API. this can be added. 
1. deck validate validates the configuration. Explain how deck can be used to backup, sync, and roll back changes that are not in git. 
1. The portal API is used to push the documentation to the portal. 
1. Merge the PR. 
1. The services should be registered in Kong and should be available in the dev portal. 
1. Go back to Insomnia and show that the request now works. 

**Tell**
1. Product management, engineering, and developers can work together using spec driven design. 
1. Developers can configure Kong and publish their API directly with Governance (workspaces) to speed up innovation while not adding risk. 

## GitOps add Governance
**Tell**
1. Our Pizza order API is available but hasa no rate limit or authentication. 
1. This would never be allowed by security. 

**Show**
1. Change orders.yaml to add rate limiting and oidc plugins. 
1. Push to Master.
1. Show Github Actions
1. Show request to the Pizza order API is rate limited and requires client id and secret in order to authenticate. 

**Tell**
1. Developers can build security and governance into the API during the design process rather than as an afterthought. 
1. Allows fast innovation without compromosing security. 

## Gitops Governance
**To Do**
- Want to show inso-cli running tests against the pizza order api as another Github action to ensure the API has authentication and rate limiting configured to show that you can automate the testing process to speed up the release of new API features. 
