# Application Installation Options in Bitrix24

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

Installing an application in Bitrix24 depends on the solution type: a Local application, a mass-market application, or a configuration solution. The scenario for each type is described in a separate article — the links are collected in the table below.

## How Installation Works

Installing an application means registering it on a specific Bitrix24 instance, rather than downloading code. Once the application is registered, the Bitrix24 authorization server begins issuing [OAuth 2.0 tokens](../oauth/index.md) to it, which the application uses to work with that Bitrix24 via the REST API.

Installing a Local or mass-market application consists of three stages:

1. **Adding the Application** — A developer creates a Local application in the *Developer resources* section of their Bitrix24, whereas a mass-market application is added in the [developer console](../../market/preparing-to-publish/how-to-add-app.md) and, after testing and moderation, published to the Bitrix24 Market.
2. **Installing on Bitrix24** — The server registers the application and issues OAuth 2.0 tokens. The installation is started by a Bitrix24 administrator or by an employee whom the administrator has allowed to install applications. A developer installs a Local application on their own Bitrix24. A mass-market application is installed by an administrator from the Bitrix24 Market or by a developer from the developer console on any Bitrix24 where the developer has administrative access.
3. **Completing Installation** — An application with an installation wizard calls the JS SDK function [BX24.installFinish()](../../sdk/bx24-js-sdk/system-functions/bx24-install-finish.md). Bitrix24 considers all other applications installed right away. Whether an application has a wizard is defined in its settings. Details can be found in the [Completing Application Installation](./installation-finish.md) article.

After installation, new versions of the application are released, and the application may be removed from Bitrix24. What happens in these cases is described in [{#T}](../app-update.md) and [{#T}](../app-uninstallation.md).

## Installation Options

The solution types and the articles about their installation are listed in the table. A configuration solution is a ready-made set of Bitrix24 configurations that operates without the REST API. Bitrix24 downloads it from the Bitrix24 Market and applies the configurations automatically.

#|
|| **Solution type** | **What it is** | **Where installation is described** ||
|| Local application | An application that works within a single Bitrix24 where it was created | [Local applications](./local-apps/index.md) ||
|| Mass-market application | An application that is published in the Bitrix24 Marketplace and installed on different Bitrix24 instances | [Mass-market applications](./mass-market-apps/index.md) ||
|| Industry CRM | A configuration solution with CRM settings: deal funnels, lead stages, custom fields, robots, and business processes | [Installing industry CRMs](./vertical-crm-installation.md) ||
|| Smart scripts | A configuration solution based on CRM robots embedded into objects outside of funnels | [Installing Smart scripts](./smart-scripts-installation.md) ||
|| Website templates | A configuration solution with a ready-made one-page or multi-page website | [Installing website templates](./site-templates-installation.md) ||
|#

[BI Builder templates](../../market/preparing-to-publish/requirements-superset.md) and other types of solutions that do not use the REST API are also published as configuration solutions.

**How to choose between a Local and a mass-market application.** A Local application fits when only one Bitrix24 needs the integration, for example your own company's or a single client's. It is created directly in that Bitrix24, without the developer console or moderation. Choose a mass-market application when the solution needs to be installed on different Bitrix24 instances and distributed through the Bitrix24 Market. It is created in the developer console and goes through moderation before publication.

{% note info "" %}

If you are planning a mass-market application, conduct your development directly in the developer console. The installation procedure for a mass-market application may differ from that of a Local application. In that case, you will need to rework the installation procedure you debugged on the Local application.

{% endnote %}
