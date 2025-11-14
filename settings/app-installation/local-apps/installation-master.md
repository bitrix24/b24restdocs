# Installation Wizard for Local Application

When a local application is added to Bitrix24 for the first time and the user, who is by definition the same Bitrix24 administrator that added the local application, accesses it, Bitrix24 initially displays the URL specified in the "Path for initial installation" field within the application slider.

The user interface implemented at this URL serves as the application's "installation wizard" with any necessary business logic. This could be a configuration form for the application, an informational interface, etc.

Additionally, this URL can be used to initialize and create the required objects in Bitrix24. For example, you can:

- set up [event handlers](../../../api-reference/events/index.md);
- register [widgets](../../../api-reference/widgets/index.md) in the necessary embedding locations;
- add a [payment system provider](../../../api-reference/pay-system/index.md);
- register an [SMS messaging provider](../../../api-reference/messageservice/index.md);
- etc.

In other words, the "installation wizard" URL can be used to perform one-time operations related to adding the application to the account, as after a successful addition, the URL specified in the "Path for initial installation" field will no longer be called.

However, your application must explicitly "notify" Bitrix24 that the installation has been successfully completed. To do this, you need to call the JS method [BX24.installFinish()](../../../sdk/bx24-js-sdk/system-functions/bx24-install-finish.md). Until this method is called, Bitrix24 considers the application not installed (not configured by the user), and each time the user navigates to the application, it will display the URL specified in the "Path for initial installation" field in the slider.

## Features of Installing a Static Local Application

Since when uploading a static application, you are uploading an archive of the entire solution rather than specifying paths to your own server, Bitrix24 uses the install.html file from the solution archive as the URL for the "installation wizard." Otherwise, the logic remains the same—you can use JS functions in this file to perform the necessary requests, and to notify about the successful completion of the "wizard," you need to call the method [BX24.installFinish()](../../../sdk/bx24-js-sdk/system-functions/bx24-install-finish.md).

If install.html is not present in the root of the archive, Bitrix24 assumes that the "installation wizard" procedure is unnecessary and immediately accesses index.html from the same archive.