# Setting Up and Using the REST API

This section contains answers to practical questions that arise during integration development:
- how to securely access the REST,
- configure different types of applications,
- optimize load.

To make your first request, check out the [Getting Started](../first-steps/index.md) section.

To find a specific REST API method, use the [Method Reference](../api-reference/index.md).

## Configure REST Calls {#start}

Review materials that explain how to form requests, encode data, and handle sequential queries.

- What a basic request looks like and how response formats differ — [How to Make a Request](how-to-call-rest-api/general-principles.md).
- How to perform sequential method calls and pass results between requests — [How to Execute a Batch of Requests](how-to-call-rest-api/batch.md).
- What to do if parameters contain special characters — [Data Encoding](how-to-call-rest-api/data-encoding.md).
- How to properly retrieve lists and work with the `start` parameter — [Features of List Methods](how-to-call-rest-api/list-methods-pecularities.md).
- Why a request stopped working after changing the domain and how to handle redirects — [Features of REST Calls When Changing the Bitrix24 Address](how-to-call-rest-api/change-domen.md).

## Perform Authorization {#auth}

Choose an authorization method — from simple webhooks to a full OAuth 2.0 cycle, set up automatic token renewal and error handling.

- How webhooks and the OAuth protocol differ — [Authorization in REST](how-to-call-rest-api/authorization.md).
- How OAuth authorization works step by step — [Complete OAuth 2.0 Authorization Protocol](oauth/index.md).
- How to perform authorization directly in the application interface or through the installation event — [Simplified Token Acquisition](oauth/simple-way.md).
- How to avoid losing access overnight and when to refresh the `refresh_token` — [Automatic Renewal of OAuth 2.0 Tokens](oauth/auto-renewal.md).
- How to deal with errors like `invalid_client`, `insufficient_scope`, and others — [Error Codes](oauth/error-codes.md).

## Install and Configure the Application {#app}

Choose the appropriate scenario and follow the instructions for local, mass-market, or configuration solutions.

- Select the suitable application option — [Application Installation Options in Bitrix24](app-installation/index.md).
- How to add a local application — [Overview of Installing Local Applications](app-installation/local-apps/index.md).
- How to create and install a mass-market solution — [Overview of Installing Mass-Market Applications](app-installation/mass-market-apps/index.md).
- When to call `installFinish` and what to check before launching — [Completing Application Installation](app-installation/installation-finish.md).
- How to publish ready-made sites, industry CRMs, and smart scenarios — [Installing Site Templates](app-installation/site-templates-installation.md), [Installing Industry CRMs](app-installation/vertical-crm-installation.md), [Installing Solutions with Smart Scenarios](app-installation/smart-scripts-installation.md).
- What data is deleted when an application is removed and how to handle the deletion event — [Uninstalling Applications](app-uninstallation.md).

## Configure Application Operation in the Cloud and On-Premise {#box}

Compare the requirements of the cloud and on-premise versions, configure the network, and expand the API if necessary.

- What is important to consider for the application to work in the on-premise version — [Differences in Using the REST API](cloud-and-on-premise/index.md).
- Why a method may be unavailable — [Versioning of Modules in On-Premise Bitrix24](cloud-and-on-premise/on-premise/versions.md).
- Which domains to open and where to get the list of IPs for event queues — [Required Network Access](cloud-and-on-premise/network-access.md).
- What to do if the corporate network is isolated and you need to replace the authorization server — [Application Authorization in Isolated On-Premise Bitrix24](cloud-and-on-premise/on-premise/custom-auth-provider.md).
- How to add your own methods and new scopes — [Adding Custom Methods to the On-Premise Bitrix24 REST API](cloud-and-on-premise/on-premise/custom-methods.md).
- What security requirements to consider during development — [Security Recommendations for Applications Using the REST API](cloud-and-on-premise/security-recommendations.md).

## Implement an Interactive Interface and Instant Events {#push}

Use Push & Pull to instantly respond to user actions in the interface.

- What interactive options are available — [Interactive Applications](interactivity/index.md).
- How to create and configure your own Push & Pull client — [Custom Push & Pull Client](interactivity/custom-push-and-pull-client.md).
- Which methods to use to get connection parameters and send push events — [Push & Pull](interactivity/push-and-pull/index.md).

## Optimize Performance Under Load {#limits}

Apply recommendations for optimizing requests, working with queues, and controlling limits.

- How to reduce the number of requests and speed up responses — [General Recommendations](performance/index.md).
- How to export thousands of items and avoid counting the total — [Retrieve Large Volumes of Data](performance/huge-data.md).
- How to build an incoming queue and protect handlers from spikes — [Incoming Event Queue](performance/queue.md).
- What limitations apply to the REST API — [Limits](performance/limits.md).