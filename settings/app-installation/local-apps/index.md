# Overview of Installing Local Applications

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

A local application operates within a single Bitrix24 instance—the one where it was created. The installation scenario determines what happens after the application form is saved. This page helps you choose a scenario and fill in the form for it. The types of local applications and the conditions for creating them are described in the article [{#T}](../../../local-integrations/local-apps.md).

> Quick links: [How to Choose a Scenario](#installation-scenarios)
>
> User documentation: [Create webhooks and apps in Bitrix24](https://helpdesk.bitrix24.com/open/21133100/)

## Connection with Other Objects

**OAuth 2.0.** Bitrix24 passes authorization data to the application, which it uses to execute methods. Retrieving and refreshing tokens is described in the [OAuth 2.0](../../oauth/index.md) section.

**Events and Widgets.** During initial setup, an application can register [event handlers](../../../api-reference/events/index.md) or add [widgets](../../../api-reference/widgets/index.md) to the Bitrix24 interface.

## Getting Started

1. Prepare the code: a ZIP archive for a static application, or a page and a handler for a server-side one. The addresses of a server-side application must start with `https://` or `http://`; use HTTPS for a production application. In the callback scenario, the handler must be running before you save the form: Bitrix24 sends the authorization data to it immediately after saving
2. Open the form: *Developer resources*, the *Common use cases* tab, then *Other > Local application*
3. Specify the name, the access permissions, and the scenario fields from the [How to Choose a Scenario](#installation-scenarios) table
4. Save the application. If you selected the [installation wizard](./installation-master.md), open the application and complete the setup. If you selected the [installation callback](./installation-callback.md), check that the handler has saved the tokens

## What to Keep in Mind

- **Access.** A local application can be created by a Bitrix24 administrator or by an employee with the permission to create applications. How to grant this permission is described in the section [How to Configure Access to Application Creation for Employees](../../../local-integrations/local-apps.md#local-app-access)
- **Application permissions.** The set of `scope` permissions is selected in the form. The installation wizard and the callback handler call methods only within this set. Where to store the secret key and tokens is described in the article [{#T}](../../../local-integrations/local-apps.md)
- **Changing the scenario.** Saving the form does not reinstall the application. If you change the *Initial installation path* or the *Application completes the installation itself* option for a saved application, click *Reinstall* in the form to apply the new scenario

## How to Choose a Scenario {#installation-scenarios}

For a server-side application, the scenario is determined by the *Initial installation path* field and the *Application completes the installation itself* option. The option is disabled by default. A static application has no such settings: the scenario depends on whether the archive root contains an `install.html` file.

#|
|| **If necessary** | **What to fill in the form** | **Open** ||
|| Show the application page inside Bitrix24 without a server of your own. Without an `install.html` file in the archive, the application is available immediately after saving | *Static* mode, a ZIP archive with an `index.html` file in the root, *Menu item text* for at least one language. The mode is available only in cloud Bitrix24 | [{#T}](../../../local-integrations/static-local-app.md) ||
|| Show the application page inside Bitrix24 and run code on your own server. The application is available immediately after saving | *Server* mode, *Your handler path*. Leave *Initial installation path* empty | [{#T}](../../../local-integrations/serverside-local-app-with-ui.md) ||
|| Show the administrator a setup form before getting started. The application is not installed until the page calls [BX24.installFinish()](../../../sdk/bx24-js-sdk/system-functions/bx24-install-finish.md) | Server-side application: *Initial installation path*, the *Application completes the installation itself* option enabled. Static application: an `install.html` file in the archive root | [{#T}](./installation-master.md) ||
|| Receive tokens on your own server and work in the background. Bitrix24 sends the authorization data in the [ONAPPINSTALL](../../../api-reference/common/events/on-app-install.md) event | *Initial installation path*, the *Application completes the installation itself* option disabled. If no interface is needed, leave the *Menu item text* fields empty | [{#T}](./installation-callback.md), example — [{#T}](../../../local-integrations/serverside-local-app-with-no-ui.md) ||
|#
