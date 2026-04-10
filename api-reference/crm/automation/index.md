# Automation and Triggers

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can execute the method: a user with access permission to modify the target object `target`

Triggers are a tool for CRM automation. Unlike Automation rules, triggers do not perform operations. Instead, they can monitor client actions and certain changes within the CRM. Triggers in CRM are available for leads, deals, estimates, invoices, and SPAs. For more details, refer to the article [Triggers in CRM](https://helpdesk.bitrix24.com/open/16628872/).

Methods for working with triggers:

#|
|| **Method** | **Description** ||
|| [crm.automation.trigger](./crm-automation-trigger.md) | Activates the Webhook trigger configured in CRM automation ||
|| [crm.automation.trigger.*](./triggers/index.md) | Methods for working with application triggers ||
|#