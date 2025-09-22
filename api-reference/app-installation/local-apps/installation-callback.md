# Installation Callback

When a local application is added with the option "Uses only API," it means that it does not have a basic interface in the left menu. Such an application can still register widgets in various embedding locations, but automatically, the item in the left menu will not be added by itself.

This is a useful scenario for applications that do not require a separate user interface, settings, etc., and whose entire business logic is implemented, for example, in the form of automation event handlers.

However, despite the lack of a user interface, such applications still need to obtain authorization tokens. This is where the callback handler comes into play, and the path to it should be specified in the "Path for initial installation" field.

Then, immediately after adding the local application, Bitrix24 will automatically call this URL and send a POST request with OAuth 2.0 authorization data, including the access token and refresh token.

It is expected that the application should store these tokens on its side to update the access token as needed in the future.

In the case of the installation callback handler, the application **must not** call the JS method [BX24.installFinish()](../../../sdk/bx24-js-sdk/system-functions/bx24-install-finish.md) as is required for local applications with the [installation wizard](./installation-master.md).

Moreover, even if an attempt is made to do so, it will not work, as this JS method belongs to a library that operates only within the frames of the application's interface in the browser, while the application's callback handler is invoked from the Bitrix24 backend. No browser is involved in this process.