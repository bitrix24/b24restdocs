# Application Installation Options in Bitrix24

When planning user scenarios to be implemented in the application, it is essential to understand how the installation process works on a specific Bitrix24 account.

First and foremost, it is necessary to distinguish between the installation of [on-premise applications](./local-apps/index.md) and [mass-market solutions](./mass-market-apps/index.md). Therefore, if you initially plan to develop a mass-market solution, it is advisable to start development by adding the application in the Developer's area instead of creating a "test local" application. Otherwise, after debugging the installation process for the local application, you may find that the installation script needs to be modified for the mass-market solution.

A separate installation scenario is implemented for so-called "configuration" solutions: [industry-specific CRMs](./vertical-crm-installation.md), packages with "[smart scripts](./smart-scripts-installation.md)", [website](./site-templates-installation.md) templates, BI constructor templates, and other types of solutions that are not REST applications but can be published in the Bitrix24 Marketplace.

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect the [MCP server](../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}