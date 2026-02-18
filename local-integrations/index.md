# Tools for Local Integrations

Local integrations are software solutions that are created and configured for a specific Bitrix24. Unlike cloud applications from the Marketplace, local tools are designed for internal company tasks and can be tailored to specific business processes.

## Typical use-cases and scenarios

Use local tools to address the following tasks:

- importing data into CRM or other Bitrix24 tools
- integration with internal systems
- simple automation related to bulk processing of customer data
- embedding custom elements into the Bitrix24 interface

{% note tip  "" %}

[Typical use-cases and scenarios](./use-cases.md)
  
{% endnote %}

## Incoming and Outgoing Webhooks

[Incoming and outgoing webhooks](./local-webhooks.md) are suitable for quick integrations and simple scenarios where complex authorization logic is not required. Incoming webhooks provide a permanent key for calling REST API methods, while outgoing webhooks send data to external systems when events occur in Bitrix24.

{% note warning "" %}

To use outgoing webhooks in the on-premise version of Bitrix24, an active license is required; outgoing webhooks are not available in demo modes.

{% endnote %}

## Local Applications

[Local applications](./local-apps.md) allow you to:

- create a custom interface
- perform server-side processing
- subscribe to events
- configure access permissions
  
There are three types of local applications:

- [Static application](./static-local-app.md) — runs in the browser on HTML/JS, does not require its own server, and is displayed as a separate page in the Bitrix24 interface.
- [Server-side application with interface](./serverside-local-app-with-ui.md) — code runs on your server, displayed as a page or embedded widget within Bitrix24.
- [Server-side application without interface](./serverside-local-app-with-no-ui.md) — runs in the background on your server, not displayed in the interface, suitable for automated tasks and synchronization.

{% note tip  "" %}

[How to fix the error "The site does not allow connection" when opening the application](./site-does-not-allow-connection.md)

{% endnote %}

## Developer's area

The [Developer's area](./developers-area.md) contains tools to simplify working with local integrations. You can access this section through the left menu of Bitrix24 by navigating to *Applications > Developer's area*.

### Ready-made scenarios

Templates for typical tasks with code examples and pre-set parameters based on webhooks and local applications.

### Integrations

A list of all created webhooks and applications with information about access permissions, events, and widgets.

### Statistics

A graph of the total number of requests and details for each integration. Helps monitor load and debug operations.

{% note tip  "" %}

- [For Developers: how to create webhooks and applications for Bitrix24](https://helpdesk.bitrix24.com/open/21133100/)
- [REST Blocking: reasons and solutions](https://helpdesk.bitrix24.com/open/21138454/)
- [How to restore the operation of blocked webhooks](https://helpdesk.bitrix24.com/open/21269128/)

{% endnote %}