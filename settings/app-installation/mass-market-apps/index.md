# Overview of mass-market application installation

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Published mass-market solutions are installed by users on their Bitrix24 from the Bitrix24 Marketplace.

Additionally, during the development stage, you can install an application from the developer console onto any Bitrix24 to which you have administrative access.

A normal scenario for creating mass-market solutions is that the developer first "creates" the application in the developer console, then writes and tests the code by installing the application under development on an available Bitrix24, and only after ensuring it is fully ready, submits the solution for moderation. More details can be found in the [corresponding section](../../../market/preparing-to-publish/how-to-add-app.md).

When developing an application, you must understand what the installation procedure will be and whether it is even necessary.

In fact, "installing an application" on a specific Bitrix24 is not a solution upload or a download (with the exception of website templates and other "configuration solutions" that work without the REST API), but rather the "registration" of the application on the Bitrix24 where the installation is occurring. This registration is required solely so that the authorization server begins issuing OAuth tokens to your application for the users of that specific Bitrix24.

Accordingly, depending on the user onboarding scenario for your application, you must choose an installation scenario.

There are 4 options for the installation process that will be used during application installation:

- Addition without an installation scenario;
- [Addition with an installation wizard](./installation-master.md);
- [Addition with a configuration wizard for a REST-only application](./rest-only-installation-master.md);
- [Addition with an installation callback call](./installation-callback.md).

The second option is most commonly chosen for mass-market applications simply because "installing" an application often requires performing certain "one-time" procedures, such as registering widget integration locations, registering event handlers, etc.

## Addition without an installation scenario

To ensure that no installation procedure as such exists and the application works immediately, it is sufficient to fill in the "Application link" field in the version card, specifying the main application URL that will subsequently be used to embed the application interface into the Bitrix24 left menu.

Similarly, in the case of a static application consisting of an archive with html/js files, you only need to have an index.html file in that archive.

In both cases, after installing the application, it will already be available to users without any "special" installation procedure.

If your application does not have a user interface, you need to disable the "Add your own page and item in the main menu" option. In this case, even though you still do not need an "installation" as such, you still need REST API tokens for further use. In this case, you cannot do without a [callback handler](./installation-callback.md), which will receive a call from Bitrix24 immediately after the application is installed.

## Continue Learning

- [{#T}](../../system-user.md)