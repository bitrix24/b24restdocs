# Overview of Installing Local Applications

Each local application can only operate on the Bitrix24 account where it was added.

In fact, the action of "adding" or "creating" a local application is already equivalent to the installation procedure. For mass-market solutions, these actions differ—first, the solution must be added to the Developer's area, and only then can it be installed on a specific Bitrix24 account. For local applications, adding and installing are the same action.

To add a local application, you need to go to the Developer resources section, select "Other," and in the opened slider, choose the scenario "Local Application."

There are three options for the installation process that will be triggered when saving a local application:

- Adding without an installation scenario;
- Adding with an installation wizard;
- Adding with a callback installation.

Most often, the first option is chosen for local applications simply because a special installation process in the form of a wizard or callback is usually more beneficial for mass-market solutions, which require a specific onboarding process for new users or a one-time interface with pre-configured user options, or a callback for saving user tokens at the time of installation on another Bitrix24 account.

A local application is added and created by the portal administrator only once. More often than not, developing a setup wizard for one's own application is an excessive use of resources. Nevertheless, such scenarios are also possible.

## Adding Without an Installation Scenario

To ensure that the installation procedure is absent and the application works immediately, it is sufficient to fill in the "Path to your handler" field, specifying the main URL of the application, which will later be used to embed the application's interface into Bitrix24's left menu.

Similarly, in the case of a static application, which consists of an archive with HTML/JS files, you only need to have an index.html file in that archive.

In both cases, after saving the local application, it will already be available to users without any installation procedure.

If your application does not have a user interface, you need to enable the "Uses only API" option. Even though you still do not need an "installation," you will still require REST API tokens for further use. In this case, you cannot do without a [callback handler](./installation-callback.md), which will receive a call from Bitrix24 immediately after the local application is added.

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect the [MCP server](../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}