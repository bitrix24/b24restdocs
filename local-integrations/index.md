# Tools for Local Integrations

Local integrations are software solutions that are created and configured for a specific Bitrix24 instance. A local integration cannot be installed on another Bitrix24 instance: its access permissions, event handlers, and widgets are configured by an employee of the same company. If the integration needs to be installed on multiple Bitrix24 instances, choose a [mass-market application](../market/index.md).

A local integration is built on two tools — webhooks and local applications. They differ in the authorization method, where the code runs, and the set of available capabilities. Parameters, responses, and errors of individual methods are described in the [REST API Reference](../api-reference/index.md).

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

## Typical Tasks for Local Integrations

Local integrations address tasks that are limited to a single Bitrix24 instance:

- Importing data into CRM or other Bitrix24 tools
- Data exchange with internal company systems
- Automation related to bulk processing of customer data
- Embedding your own elements into the Bitrix24 interface

Ready-made method sequences for these tasks are collected in the article [Local Integrations: Use Case Scenarios](./use-cases.md).

## How to Get Started {#how-to-start}

1. Define the integration task and choose a tool in the [table](#choose-tool).
2. Check the [access permissions](#rights): the required `scope` and the permissions of the employee on whose behalf the integration will access Bitrix24.
3. Create the integration in the [Developer resources](./developers-area.md) section — *Applications > Developer resources*. For a server-side application, place the handler on your server in advance and make it available over HTTPS.
4. Test the first request in the request generator or follow the [How to Make Your First API Request](../first-steps/first-rest-api-call.md) guide.
5. Review a ready-made scenario from the article [Local Integrations: Use Case Scenarios](./use-cases.md) and build your own sequence of methods.

## How to Choose a Tool {#choose-tool}

#|
|| **Tool** | **Authorization** | **Capabilities** | **When to Choose** ||
|| [Incoming webhook](./local-webhooks.md) | Secret code in the request URL | Calls methods on behalf of the employee who created the webhook | An external system or script calls Bitrix24 methods ||
|| [Outgoing webhook](./local-webhooks.md) | Token that Bitrix24 passes to the handler in the `application_token` field | Sends a Bitrix24 event to your handler URL | An external system reacts to data changes in Bitrix24 ||
|| [Static application](./static-local-app.md) | Current employee's authorization, retrieved automatically by the JS SDK | Calls methods and displays its own page in the interface. Does not receive events | You need an interface inside Bitrix24 without your own server ||
|| [Server-side application with interface](./serverside-local-app-with-ui.md) | Simplified OAuth 2.0 in the context of the current employee | Calls methods, displays a page and [widgets](../api-reference/widgets/index.md), receives events | You need server-side processing and an interface inside Bitrix24 ||
|| [Server-side application without interface](./serverside-local-app-with-no-ui.md) | Full OAuth 2.0 protocol, tokens are retained by the [ONAPPINSTALL](../api-reference/common/events/on-app-install.md) event handler | Calls methods and receives events, is not displayed in the interface | Background synchronization and automated tasks ||
|#

## Incoming and Outgoing Webhooks

[Webhooks](./local-webhooks.md) are suitable for quick integrations where complex authorization logic is not required.

An incoming webhook is a secret code that an employee retrieves in the Bitrix24 interface and inserts into the request URL. An incoming webhook can be limited by an expiration date. An outgoing webhook works in the opposite direction: when an event occurs, Bitrix24 sends the data to your handler URL.

Some methods are not available through webhooks because their logic requires an application context. These include methods for embedding applications into the Bitrix24 interface, telephony events, and some chat bot events. For such scenarios, a local application is required.

{% note warning "" %}

To use outgoing webhooks in the on-premise version of Bitrix24, an active license is required; outgoing webhooks are not available in demo modes.

{% endnote %}

## Local Applications

[Local applications](./local-apps.md) allow you to:

- Create a custom interface
- Perform server-side processing
- Subscribe to events
- Configure access permissions

Applications are divided into static and server-side. A static application runs in the browser using HTML and JS, while a server-side application executes code on your server. Server-side applications differ in whether they have an interface, so there are three options:

- [Static application](./static-local-app.md) — runs in the browser, does not require its own server, and is displayed as a separate page in the Bitrix24 interface
- [Server-side application with interface](./serverside-local-app-with-ui.md) — code runs on your server and is displayed as a page or an [embedded widget](../api-reference/widgets/index.md) within Bitrix24
- [Server-side application without interface](./serverside-local-app-with-no-ui.md) — runs in the background on your server, is not displayed in the interface, and is suitable for automated tasks and synchronization

{% note info "" %}

A static application does not receive Bitrix24 events — it has no server-side handler for Bitrix24 to pass them to. If the integration needs to react to data changes, choose a server-side application or an outgoing webhook

{% endnote %}

## Access Permissions and Security {#rights}

- **Permissions.** The integration works within the permissions of the employee who created it and the list of [scopes](../api-reference/scopes/permissions.md) selected at creation. A method returns an error if the required `scope` is missing or the employee has no permissions for the object.
- **REST API access.** A local integration works only if Bitrix24 has [access to the REST API](../first-steps/access-to-rest-api.md) — through a Market subscription, Trial mode, or an NFR key.
- **Protocol.** Webhook requests are performed over HTTPS only. The handler of a server-side application must also be available over HTTPS before you add the application to Bitrix24.
- **Secret code.** A leaked URL grants access to Bitrix24 within the webhook's permissions until the webhook is deleted or expires. Do not pass the webhook URL to external systems and do not publish it in client-side code.
- **Deletion.** An integration can be deleted by a Bitrix24 administrator or by the employee who created it. Secret codes of other users' webhooks are not available even to an administrator.

## Developer Resources

The [Developer resources](./developers-area.md) section brings together tools for working with local integrations. You can open it through the left menu in Bitrix24 by navigating to *Applications > Developer resources*.

### Ready-made Scenarios

Templates for typical tasks with code examples and pre-set parameters based on webhooks and local applications. The request generator is also located here: it selects method parameters, runs the request, and provides a ready-made code sample.

### Integrations

A list of all created webhooks and applications with information about access permissions, events, and widgets. From the list, you can open the integration settings or delete it.

### Statistics

A graph showing the total number of requests and details for each integration. This helps monitor load and identify the integration that caused REST to be blocked.

{% note tip "" %}

User documentation:

- [For Developers: how to create webhooks and applications for Bitrix24](https://helpdesk.bitrix24.com/open/21133100/)
- [REST Blocking: reasons and solutions](https://helpdesk.bitrix24.com/open/21138454/)
- [How to restore the operation of blocked webhooks](https://helpdesk.bitrix24.com/open/21269128/)

{% endnote %}

## Continue Learning

- [{#T}](./use-cases.md)
- [{#T}](./local-webhooks.md)
- [{#T}](./local-apps.md)
- [{#T}](../first-steps/first-rest-api-call.md)
- [{#T}](../settings/cloud-and-on-premise/security-recommendations.md)
