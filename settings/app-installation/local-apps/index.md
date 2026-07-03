# Overview of Installing Local Applications

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

A local application operates within a single Bitrix24 instance—the one where it was created. It can display reports, exchange data with an external system, or add its own interface items.

After saving, the application is added to Bitrix24, and further installation depends on the selected scenario. Choose a scenario before saving:

- Make the application immediately available to users
- Show the initial setup page to the administrator first
- Pass authorization data to the application server

> Quick links: [How to Choose a Scenario](#installation-scenarios)
>
> User documentation: [Developer resources: How to create webhooks and applications for Bitrix24](https://helpdesk.bitrix24.com/open/20886106/#2)

## Connection with Other Objects

**OAuth 2.0.** Bitrix24 passes authorization data to the application, which it uses to execute methods. Retrieving and refreshing tokens is described in the [OAuth 2.0](../../oauth/index.md) section.

**Events and Widgets.** During initial setup, an application can register [event handlers](../../../api-reference/events/index.md) or add [widgets](../../../api-reference/widgets/index.md) to the Bitrix24 interface.

## Getting Started

1. Determine whether the application requires an interface within Bitrix24
2. Prepare the application page, server handler, or a ZIP archive containing static files
3. Select an option from the [How to Choose a Scenario](#installation-scenarios) table
4. Open the local application form following the [user instructions](https://helpdesk.bitrix24.com/open/20886106/#2)
5. Specify a name, select permissions, and fill in the fields for the chosen scenario
6. Save the application and perform the initial setup if required

## How to Choose a Scenario {#installation-scenarios}

#|
|| **If necessary** | **Open** ||
|| Make the application immediately available to users | [Server-side local application with a user interface](../../../local-integrations/serverside-local-app-with-ui.md) or [Static local application](../../../local-integrations/static-local-app.md) ||
|| Configure the application first | [Local application installation wizard](./installation-master.md) ||
|| Pass authorization data to the application server | [Installation callback](./installation-callback.md) ||
|#
