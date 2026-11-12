# Mass-Market Application Installation Overview

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

Before it can function, a mass-market application must be installed on a specific Bitrix24 instance. There are four installation options for this. They differ in what the user does when launching the application for the first time.

## Mass-Market Application Workflow

What installation is and why it is required is described in the [{#T}](../index.md) overview. Below is a summary of what is specific to mass-market applications.

Typically, a developer creates an application in the developer console, writes the code, and tests it by installing the application from the console onto a Bitrix24 where the developer has administrative access. Only after the application has been tested does the developer submit it for moderation. Once published, users install the solution from the Bitrix24 Market. For more details on preparing for publication, see the [{#T}](../../../market/preparing-to-publish/how-to-add-app.md) section.

Decide in advance what the installation procedure will be and whether it is necessary at all.

## Installation Process Options

Installation options:

- **[Addition without an installation scenario](#no-install-scenario)** — the application begins working immediately after installation. This is suitable when the application does not require one-time configurations during installation.
- **[Addition with an installation wizard](./installation-master.md)** — during installation, Bitrix24 opens your interface, where one-time operations can be performed, such as registering widgets, subscribing to events, and more. This is suitable for applications with a user interface that require initial configuration.
- **[Addition with an installation callback](./installation-callback.md)** — after installation, Bitrix24 sends OAuth tokens to your handler. This is suitable for applications without an interface, where all logic operates within event handlers.
- **[Addition with a configuration wizard for REST-only applications](./rest-only-installation-master.md)** — a built-in Bitrix24 form for applications without their own interface. This is suitable when simple configurations are required and data from a callback handler is insufficient.

Mass-market applications most often need an installation wizard: it is where widget integration points are registered and event subscriptions are created.

## Adding Without an Installation Scenario {#no-install-scenario}

For the application to work right away, without an installation procedure, fill in the *App URL* field in the version card. Enter the main application URL here: this is the address used to embed the application interface into the Bitrix24 left menu.

For a static application—an archive containing HTML and JS files—simply include an `index.html` file in the archive.

With either a URL or a static archive, the application is available to users right after installation.

If the application does not have a user interface, disable the *Add your page and item to the main menu* option. Such an application does not need an installation procedure, but it does need REST API tokens. The application receives them through a [callback handler](./installation-callback.md): Bitrix24 calls it right after installation.

## Continue Learning

- [{#T}](../../app-update.md)
- [{#T}](../../system-user.md)
- [{#T}](../../oauth/index.md)
- [{#T}](../../../market/preparing-to-publish/how-to-add-app.md)
