# Bitrix24 Cloud and Self-Hosted

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

Bitrix24 operates as both a cloud service and as self-hosted installations on a client's server. A REST application uses the same methods, events, and OAuth authorization in both versions. This section helps prepare the application to work with the self-hosted version: accounting for its differences, opening the necessary network access on the client side, and complying with security requirements.

## How the Self-Hosted Version Differs from the Cloud

| What differs | Cloud | Self-hosted | Details |
|---|---|---|---|
| REST version | Always the latest | Updates are released later than in the cloud, and the owner may not install them | [{#T}](on-premise/versions.md) |
| Set of modules | Bitrix24 installs modules and updates | The owner may not install or may disable a module, for example telephony. The methods and events of such a module will be unavailable | [{#T}](on-premise/index.md) |
| Event delivery | Through the event queue in the Bitrix24 cloud | Also through the cloud event queue. If network access to it is closed, events do not arrive | [{#T}](network-access.md) |
| Token validation | The `oauth.bitrix.info` authorization server | The same server. If access to it is closed, REST does not work. Tokens must be refreshed with a request directly to `oauth.bitrix.info` | [{#T}](on-premise/index.md) |
| Network access | No configuration required | The client's network administrator opens connections to Bitrix24 servers | [{#T}](network-access.md) |
| Isolated network | Not applicable | You can connect your own authorization and event mechanism without `oauth.bitrix.info`. This is a last resort: you are responsible for the security of authorization | [{#T}](on-premise/custom-auth-provider.md) |
| Custom REST methods | Not available | You can add your own methods and scopes | [{#T}](on-premise/custom-methods.md) |

## When You Need This Section

- The application will be installed in a self-hosted Bitrix24 — check the availability of methods and events during installation as described in [{#T}](on-premise/index.md)
- You are deploying a solution for a client with a firewall — agree on the rules from [{#T}](network-access.md) with the network administrator
- You are preparing the application for publication or release — go through the checklist in [{#T}](security-recommendations.md). The recommendations apply to both the cloud and self-hosted versions
- The application works only with cloud Bitrix24 — you do not need to account for the self-hosted differences or configure network access

## All Section Materials

#|
|| **Material** | **Description** ||
|| [REST API features in the boxed version](on-premise/index.md) | How the boxed version differs when working with methods, events, and authorization. Module versioning, isolated box, and adding your own methods ||
|| [Required network access](network-access.md) | Which inbound and outbound requests to open for the application to work with the boxed Bitrix24 ||
|| [Security recommendations for REST API applications](security-recommendations.md) | Working with tokens, data validation, and communication channel protection ||
|#

## Continue Learning

- [{#T}](on-premise/versions.md)
- [{#T}](on-premise/custom-auth-provider.md)
- [{#T}](on-premise/custom-methods.md)
- [{#T}](../app-installation/index.md)
