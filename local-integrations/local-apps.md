# Local Applications

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

A local application is created and configured on a specific Bitrix24 — it works only there and cannot be installed on another Bitrix24. An application can show its own page in the interface, exchange data with an external system, or receive Bitrix24 events — the set of capabilities depends on the type.

Applications are divided into static and server-side ones by where the code runs. Server-side applications differ in whether they have an interface, so there are three options:

- static — the code runs in the browser, no server of your own is needed
- server-side with an interface — the code runs on your server, and the application shows a page inside Bitrix24
- server-side without an interface — the code runs on your server, and the application does not appear in the Bitrix24 interface

This page helps you choose the type of application and create it on your own Bitrix24. The development process and a sample walkthrough for each type are described on its own page.

> Quick links: [How to Choose the Application Type](#choose-app)
>
> User documentation: [Create Webhooks and Apps in Bitrix24](https://helpdesk.bitrix24.com/open/21133100/)

## Connection with Other Objects

**Developer resources.** Local applications are created and found in the *Applications > Developer resources* section. What else the section holds is described in the [{#T}](./developers-area.md) article.

**OAuth 2.0.** A server-side application accesses the REST API on behalf of an employee over the OAuth 2.0 protocol. How to retrieve and refresh tokens is described in the [{#T}](../settings/oauth/index.md) article.

**Events and widgets.** A server-side application subscribes to [events](../api-reference/events/index.md) and adds [widgets](../api-reference/widgets/index.md) to the Bitrix24 interface.

## How to Get Started

1. Check the conditions. A local application works only if Bitrix24 has [access to the REST API](../first-steps/access-to-rest-api.md) — under a Marketplace subscription, in trial mode, or with an NFR key. The application can be created by a Bitrix24 administrator or by an employee who has been granted the permission to install applications.

2. Decide whether the application needs an interface inside Bitrix24 and a server of its own. This determines the type of application — select it in the [How to Choose the Application Type](#choose-app) table.

3. Prepare the code: for a static application, an archive with a page in HTML and JS; for a server-side one, a page or a handler available over HTTPS before you add the application to Bitrix24.

4. Open the local application form: *Applications > Developer resources*, the *Ready-made scenarios* tab, then *Other > Local application*.

5. Fill in the form: the name, the access permissions, and the fields of the selected type — the archive with the page, the handler address, or the initial installation address.

6. Save the application. It will appear in the *Applications > Developer resources > Integrations* list.

7. Enable the *Application uses API only* option if the application needs no interface, and store the application code and the secret key in the code on your server — Bitrix24 shows them after the form is saved.

## What to Keep in Mind

- **Authorization.** A static application runs inside the Bitrix24 interface, and the JS SDK retrieves the authorization of the employee who opened it automatically. A server-side application with an interface uses a simplified variant of OAuth 2.0: the application acts on behalf of the employee who opened it, and Bitrix24 passes the tokens to the application page in a POST request — there is no need to request them separately. A server-side application without an interface implements the full OAuth 2.0 protocol: it retains the tokens itself in the [ONAPPINSTALL](../api-reference/common/events/on-app-install.md) event handler and refreshes them itself.
- **Secrets.** Keep the application code, the secret key, and the tokens obtained with them on your server. Do not place them in client-side code that loads in the browser, and do not retain them in a repository.
- **Permissions.** The set of application permissions — `scope` — is selected at creation time. The application works within the selected `scope` and the permissions of the employee who created it: a method returns an error if the required `scope` is missing or the employee has no permissions for the object. The permission codes are listed in the [{#T}](../api-reference/scopes/permissions.md) article.
- **Events.** A static application does not receive events: it has no server-side handler Bitrix24 could pass them to. If the application has to react to data changes, choose a server-side one or an [outgoing webhook](./local-webhooks.md).

## How to Choose the Application Type {#choose-app}

#|
|| **If You Need To** | **Open** ||
|| Show your own page inside Bitrix24 without a server of your own | [{#T}](./static-local-app.md) ||
|| Show your own page inside Bitrix24 and run the code on your server | [{#T}](./serverside-local-app-with-ui.md) ||
|| Work in the background without an interface: data synchronization and event handling | [{#T}](./serverside-local-app-with-no-ui.md) ||
|#

## Continue Your Exploration

- [{#T}](./developers-area.md)
- [{#T}](./local-webhooks.md)
- [{#T}](./use-cases.md)
- [{#T}](../settings/app-installation/local-apps/index.md)