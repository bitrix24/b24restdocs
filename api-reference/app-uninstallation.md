# Deleting Applications

## Local Applications

The characteristic of local applications is that they are primarily intended for internal automation. In other words, the developer of a local application is often not an external party, but rather an employee or department of the company using its Bitrix24.

Therefore, deleting a local application is usually not an operation that requires automation in the application's code.

However, you have the option to add an event handler [OnAppUninstall](./common/events/on-app-uninstall.md) in the local application. If such a handler exists, Bitrix24 will call this handler upon the application's deletion.

When a local application is deleted from Bitrix24, the following are automatically removed:

- Event handlers registered by the application [event handlers](./events/index.md);
- Handlers registered by the application [widgets](./widgets/index.md), including custom field types;
- Application [storage](./entity/index.md) created by the application;
- Application [settings](./common/settings/index.md) created by the application;
- Open line [connectors](./imopenlines/imconnector/index.md) registered by the application;
- Payment [systems](./pay-system/index.md) registered by the application;
- Cash [registers](./sale/cashbox/index.md) registered by the application.

It is not possible to cancel the deletion of a local application. Adding a local application, even with the same URLs, will effectively create a new application with a new pair of client_id/client_secret for [OAuth authorization](./oauth/index.md).

## Mass-Market Applications

In contrast to local applications, it is very important for mass-market solutions to be aware of the fact that an application has been deleted from a specific Bitrix24. Especially if the application has implemented mechanisms for updating authorization tokens or business logic that requires periodic access to Bitrix24.

If the application is deleted from a specific Bitrix24, the saved tokens will no longer be valid, and attempts to access such a Bitrix24 will only create unnecessary load on the application's servers.

To receive information about the deletion, the application must add an event handler [OnAppUninstall](./common/events/on-app-uninstall.md). If such a handler exists, Bitrix24 will call this handler upon the application's deletion.

When an application is deleted from Bitrix24, the following are automatically removed:

- Event handlers registered by the application [event handlers](./events/index.md);
- Handlers registered by the application [widgets](./widgets/index.md), including custom field types;
- Open line [connectors](./imopenlines/imconnector/index.md) registered by the application;
- Payment [systems](./pay-system/index.md) registered by the application;
- Cash [registers](./sale/cashbox/index.md) registered by the application.

Before deleting the application, Bitrix24 requests confirmation and offers the option "Delete application settings and data." If this option is not enabled, despite the deletion of the application, the following will remain in Bitrix24:

- Application [storage](./entity/index.md) created by the application;
- Application [settings](./common/settings/index.md) created by the application;

It is not possible to cancel the deletion of the application. However, it is possible to reinstall the application on the same Bitrix24. In this case, the application will regain access to the undeleted storages and settings.