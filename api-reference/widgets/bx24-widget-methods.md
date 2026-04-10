# Methods of BX24 SDK for Widgets

In widgets, you can use the JS methods `BX24.openApplication`, `BX24.openPath`, and `BX24.closeApplication`. These methods help open the application interface or standard Bitrix24 pages directly from the embedded widget without calling REST methods.

For a detailed description of the syntax, parameters, and callback functions, refer to the section [Additional Functions of BX24.js](../../sdk/bx24-js-sdk/additional-functions/index.md).

> Quick navigation: [all methods](#all-methods)

## Open Interface in Slider with BX24.openApplication

Use `BX24.openApplication` when you need to open your application's interface in a separate slider and pass arbitrary parameters to it.

This method is used for application settings, user dialogs, and displaying additional information over the current interface. If the widget interface is limited by the iframe size, the slider opens over the current page and allows you to show a separate application interface without the constraints of the original embedding area.

The method operates on the interface side and does not replace the REST API. Its purpose is to manage the application's interface itself. More details can be found in the method description [BX24.openApplication](../../sdk/bx24-js-sdk/additional-functions/bx24-open-application.md).

## Open Page with BX24.openPath

Use `BX24.openPath` when you need to open a standard Bitrix24 page, such as a CRM card, task, or user profile.

This method allows you to avoid duplicating the standard Bitrix24 interface within the application. Its purpose is navigation within the interface, not retrieving data from the page directly.

If you need to refresh data in the widget after closing the standard page, combine `BX24.openPath` with [event handling](../events/index.md) and [interactive interaction](../../settings/interactivity/index.md).

More details can be found in the method description [BX24.openPath](../../sdk/bx24-js-sdk/additional-functions/bx24-open-path.md).

## Close Application Window with BX24.closeApplication

Use `BX24.closeApplication` when you need to close the application window that was previously opened via `BX24.openApplication`.

This method operates on the interface side and manages only the application window. It is not intended for closing arbitrary standard Bitrix24 sliders.

More details can be found in the method description [BX24.closeApplication](../../sdk/bx24-js-sdk/additional-functions/bx24-close-application.md).

## Overview of Methods {#all-methods}

> Scope: [`placement`](../scopes/permissions.md)
>
> Who can execute the method: any user

#|
|| **Method** | **Description** ||
|| [BX24.openApplication](../../sdk/bx24-js-sdk/additional-functions/bx24-open-application.md) | Opens the application interface in a separate slider and passes parameters to it. ||
|| [BX24.openPath](../../sdk/bx24-js-sdk/additional-functions/bx24-open-path.md) | Opens a standard Bitrix24 page in a slider for navigating to ready objects and sections. ||
|| [BX24.closeApplication](../../sdk/bx24-js-sdk/additional-functions/bx24-close-application.md) | Closes the application window opened via `BX24.openApplication`. ||
|#