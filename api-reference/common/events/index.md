# Event List

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

{% endnote %}

{% note info " " %}

**Scope**: [`basic`](../../scopes/permissions.md) | **Who can subscribe**: `any user`

{% endnote %}

#| 
|| **Method** | **Description** ||
|| [onAppInstall](./on-app-install.md) | This event is triggered immediately after the application is successfully installed on Bitrix24. ||
|| [onAppUpdate](./on-app-update.md) | This event is triggered when the application is updated. ||
|| [onAppUninstall](./on-app-uninstall.md) | This event is triggered when the application is uninstalled. ||
|| [onAppMethodConfirm](./on-app-method-confirm.md) | This event is triggered when a decision is received from the account administrator regarding a request to use methods that require confirmation. ||
|| [onAppPayment](./on-app-payment.md) | This event is triggered when the application is paid for. ||
|| [onAppUserAdd](./on-user-add.md) | This event is triggered when a user is added to Bitrix24. ||
|#