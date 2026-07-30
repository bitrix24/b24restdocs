# Mass-Market Application Installation Overview

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Before it can function, a mass-market application must be installed on a specific Bitrix24 instance. This page explains the four available installation process options and how they differ. After reading this, you will be able to select the most suitable installation method for your application.

This page provides an overview and selection criteria. For step-by-step configuration of each option, see the child pages via the links below.

## Mass-Market Application Workflow

What installation is and why it is required is described in the [{#T}](../index.md) overview. Below is a summary of what is specific to mass-market applications.

The standard workflow is as follows: a developer creates an application in the Developer resources, writes and tests the code by installing the application from the developer portal onto an available Bitrix24 instance with administrative access, and only after verifying readiness submits the solution for moderation. Once published, users install the solution from the Bitrix24 Market. For more details on preparing for publication, see the [{#T}](../../../market/preparing-to-publish/how-to-add-app.md) section.

The installation option is chosen based on the user onboarding scenario. Determine in advance what the installation procedure will be and whether it is necessary at all.

## Installation Process Options

There are four installation process options:

- **Addition without an installation scenario** — the application begins working immediately after installation. This is suitable when the application does not require one-time configurations during installation.
- **[Addition with an installation wizard](./installation-master.md)** — during installation, Bitrix24 opens your interface, where one-time operations can be performed, such as registering widgets, subscribing to events, and more. This is suitable for applications with a user interface that require initial configuration.
- **[Addition with a configuration wizard for REST-only applications](./rest-only-installation-master.md)** — a built-in Bitrix24 form for applications without their own interface. This is suitable when simple configurations are required and data from a callback handler is insufficient.
- **[Addition with an installation callback](./installation-callback.md)** — after installation, Bitrix24 sends OAuth tokens to your handler. This is suitable for applications without an interface, where all logic operates within event handlers.

An installation wizard is most commonly chosen for mass-market applications. During installation, it is usually necessary to perform one-time operations, such as registering widget integration points, subscribing to events, and more.

The three options with dedicated pages are covered in detail via the links in the list above. The "Addition without an installation scenario" option is described on this page.

## Adding Without an Installation Scenario

To bypass the installation procedure so that the application is immediately functional, fill in the "App URL" field in the version card. Enter the main application URL here; this is the address used to embed the application interface into the Bitrix24 left menu.

For a static application—an archive containing HTML and JS files—simply include an `index.html` file in the archive.

In both cases, once installed, the application is immediately available to users without a separate installation procedure.

If the application does not have a user interface, disable the "Add your own page and item to the main menu" option. In this case, installation is not required, but the application still needs Bitrix24 REST API tokens. A [callback handler](./installation-callback.md), which Bitrix24 calls immediately after installation, will help you obtain them.

## Continue Learning

- [Mass-Market Application Installation Wizard](./installation-master.md) — how to perform one-time configurations during application installation
- [Installation Callback](./installation-callback.md) — how to obtain OAuth tokens for an application without an interface
- [{#T}](../../system-user.md)
