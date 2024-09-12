# Security in Handlers

The application developer in event handlers must ensure that the handler is being called by Bitrix24 and not by malicious actors. To achieve this, Bitrix24 passes an additional parameter **application_token** when invoking handlers.

The first time, the parameter is sent to the event handler [`OnAppInstall`](../common/events/on-app-install.md) along with the authorization data of the user who installed the application. Using this authorization data, the `OnAppInstall` event handler can verify the validity of the received **access_token** and then store the application_token for future reference, allowing it to compare the received application_token with the stored one in its other event handlers.

This is particularly relevant in the event handler [`OnAppUninstall`](../common/events/on-app-uninstall.md), as no authorization data is passed to it (the application has already been removed from Bitrix24). Therefore, in the case of OnAppUninstall, verifying the application_token against the stored value becomes the only way to ensure that the event handler was indeed called by Bitrix24.

## Continue Learning

- [{#T}](./events.md)
- [{#T}](./event-bind.md)
- [{#T}](./event-get.md)
- [{#T}](./event-unbind.md)
- [{#T}](./offline-events.md)
- [{#T}](./event-offline-list.md)
- [{#T}](./event-offline-get.md)
- [{#T}](./event-offline-clear.md)
- [{#T}](./event-offline-error.md)
- [{#T}](./on-offline-event.md)