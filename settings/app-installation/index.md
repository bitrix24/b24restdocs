# Application Installation Options in Bitrix24

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

This Page helps you choose an application installation scenario in Bitrix24 and explains the differences between installing Local applications, mass-market applications, and configuration solutions. After reading this, you will understand the installation stages, who initiates them, and which documentation section to open for your chosen solution type.

This page does not cover the configuration of specific scenarios — separate articles dedicated to them are linked below.

## How Installation Works

Installing an application means registering it on a specific Bitrix24 instance, rather than uploading or downloading code. Once registered, the Bitrix24 authorization server begins issuing OAuth 2.0 tokens to the application, which it uses to operate via the Bitrix24 REST API.

The process consists of three stages:

1. **Adding the Application** — A developer creates a Local application in the Developer resources section of their Bitrix24, whereas a mass-market application is added in the [developer console](../../market/preparing-to-publish/how-to-add-app.md) and published to the Bitrix24 Market.
2. **Installing on Bitrix24** — The server registers the application and issues OAuth 2.0 tokens. The installation is performed by a user with administrator rights. A Local application is installed by the developer on their own Bitrix24; a mass-market application is installed by an administrator from the Bitrix24 Market or by a developer from the developer console on any Bitrix24 with administrative access.
3. **Completing Installation** — An application without an interface is installed automatically; an application with an interface and a setup wizard completes the installation using the `installFinish` method. Details can be found in the [Completing Application Installation](./installation-finish.md) article.

## Installation Options

The installation scenario depends on the solution type. There are two types of solutions:

- mass-market application — operates via the Bitrix24 REST API, is published in the Bitrix24 Market, and is installed on various Bitrix24 instances.
- configuration solution — a ready-made set of Bitrix24 configurations that operates without the Bitrix24 REST API.

Bitrix24 downloads a configuration solution from the developer console and applies its settings automatically.

#|
|| **Solution type** | **What it is** | **Where installation is described** ||
|| Local application | An application that works within a single Bitrix24 where it was created | [Local applications](./local-apps/index.md) ||
|| Mass-market application | An application that is published in the Bitrix24 Marketplace and installed on different Bitrix24 instances | [Mass-market applications](./mass-market-apps/index.md) ||
|| Industry CRM | A configuration solution with CRM settings: deal funnels, lead stages, custom fields, robots, and business processes | [Installing industry CRMs](./vertical-crm-installation.md) ||
|| Smart scripts | A configuration solution based on CRM robots embedded into objects outside of funnels | [Installing Smart scripts](./smart-scripts-installation.md) ||
|| Website templates | A configuration solution with a ready-made one-page or multi-page website | [Installing website templates](./site-templates-installation.md) ||
|#

In addition to the above, [BI Builder templates](../../market/preparing-to-publish/requirements-superset.md) and other types of solutions that do not use the REST API are published as configuration solutions.

{% note info "" %}

If you are planning a mass-market solution, conduct your development directly in the developer console. The installation scenario for a mass-market solution may differ from a Local application, and an installation debugged on a Local application will have to be redone.

{% endnote %}
