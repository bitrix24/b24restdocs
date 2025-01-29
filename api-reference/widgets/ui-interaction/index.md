# Interaction with UI from Widgets

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- information about bx24-js, a link to the section with all methods, a block about useful functions and system dialogs with links to the sections below
- we explain that some of the widgets support special methods for interactive interaction with B24. For example, the call card, CRM card, and BACKGROUND_PLACEMENT.

{% endnote %}

{% endif %}

This section describes the methods that enable the integration of third-party applications into the Bitrix24 interface. The unique aspect of these methods is that they do not make a physical request to the server but work with the page where the application is embedded. Data exchange occurs via `postMessage`.

## Overview of Methods

> Scope: [`placement`](../../scopes/permissions.md)

#| 
|| **Method** | **Description** ||
|| [BX24.placement.info](bx24-placement-info.md) | Retrieves information about the context of the call ||
|| [BX24.placement.getInterface](bx24-placement-get-interface.md) | Retrieves information about the JS interface of the current embedding location: a list of possible commands and events ||
|| [BX24.placement.call](bx24-placement-call.md) | Calls a registered interface command ||
|| [BX24.placement.bindEvent](bx24-placement-bind-event.md) | Sets an event handler for the interface || 
|#