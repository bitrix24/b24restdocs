# App deletion

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

## Local apps

The specific feature of local apps is that they are intended, first and foremost, for internal automation. In other words, the developer of a local app is most often not a third party, but an employee or a department of the company using its Bitrix24.

Therefore, the deletion of a local app is most often not an operation whose reaction needs to be automated within the app code.

Nevertheless, you have the opportunity to add an [OnAppUninstall](../api-reference/common/events/on-app-uninstall.md) event handler in a local app. If such a handler exists, Bitrix24 will call this handler when the app is deleted.

When a local app is deleted, the following are automatically deleted from Bitrix24:

- [Event handlers](../api-reference/events/index.md) registered by the app;
- [Widget](../api-reference/widgets/index.md) handlers registered by the app, including custom field types;
- [App data stores](../api-reference/entity/index.md) created by the app;
- [Configurations](../api-reference/common/settings/index.md) created by the app;
- [Open Channels connectors](../api-reference/imopenlines/imconnector/index.md) registered by the app;
- [Payment systems](../api-reference/pay-system/index.md) registered by the app;
- [Cash registers](../api-reference/sale/cashbox/index.md) registered by the app.

It is not possible to undo the deletion of a local app. Adding a local app, even with the same URLs, will actually create a new app with a new `client_id`/`client_secret` pair for [OAuth authorization](./oauth/index.md).

## Mass-market applications

Unlike local applications, for mass-market solutions it is very important to know if an application has been uninstalled from a specific Bitrix24. This is especially true if the application implements authorization token refresh mechanisms or business logic that requires contacting Bitrix24 periodically.

If an application is uninstalled from a specific Bitrix24, the saved tokens will no longer work; furthermore, attempts to contact such a Bitrix24 will simply create unnecessary load on the application servers.

To receive information about uninstallation, the application must add an [OnAppUninstall](../api-reference/common/events/on-app-uninstall.md) event handler. If such a handler exists, Bitrix24 will call it when the application is uninstalled.

When an application is uninstalled from Bitrix24, the following are automatically deleted:

- [Event handlers](../api-reference/events/index.md) registered by the application;
- [Widget](../api-reference/widgets/index.md) handlers registered by the application, including custom field types;
- [Open Channels connectors](../api-reference/imopenlines/imconnector/index.md) registered by the application;
- [Payment systems](../api-reference/pay-system/index.md) registered by the application;
- [Cash registers](../api-reference/sale/cashbox/index.md) registered by the application.

Before uninstalling an application, Bitrix24 requests confirmation and offers the "Delete application settings and data" option. If this option is not selected, the following will remain in Bitrix24 despite the application being uninstalled:

- [Application data stores](../api-reference/entity/index.md) created by the application;
- [Configurations](../api-reference/common/settings/index.md) created by the application.

An application uninstallation cannot be canceled. However, the application can be reinstalled on the same Bitrix24. In this case, the application will once again gain access to the non-deleted data stores and configurations.

## Continue Learning

- [{#T}](system-user.md)