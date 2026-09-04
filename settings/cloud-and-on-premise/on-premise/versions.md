# Versioning of Modules in On-Premise Bitrix24

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

When developing applications, consider that REST methods and events available in Bitrix24 Cloud may be unavailable in specific self-hosted installations.

This happens for two reasons:

- updates for the self-hosted version are released later than cloud updates because they require separate compatibility testing
- updates are installed by the administrator of a specific self-hosted Bitrix24, so an installation may not be running the latest module versions

Before installing an application, check the availability of the required REST API tools in a specific Bitrix24 account:

1. Retrieve the list of available methods using the [methods](../../../api-reference/common/system/methods.md) method. To check one method, use [method.get](../../../api-reference/common/system/method-get.md): it shows whether the method exists in this Bitrix24 and whether it is available with the current application permissions
2. Retrieve the list of available events using the [events](../../../api-reference/events/events.md) method. The method works in the context of application authorization and returns events that can be used in Bitrix24
3. Compare the result with the methods and events required by your application. If the required method or event is missing, check the version of the module the REST API tool belongs to

The list of versions for all modules of a self-hosted Bitrix24 is available in the administrative section: *Settings > Product Settings > Modules*.

## What to Do If a Method or Event Is Unavailable

If you do not have administrative access to the self-hosted Bitrix24, provide the administrator with:

- the name of the unavailable REST method or event
- the name of the module the method or event belongs to
- the minimum module version, if it is specified in the documentation for the method, event, or section

The administrator should update the required module or the entire self-hosted installation to the latest version. Updates are installed through SiteUpdate, the built-in update system in the Bitrix24 administrative section: *Marketplace > Platform Update*. In the *Install updates* section, the administrator can install recommended updates, and on the *Updates* tab, select specific updates. For details about updating a self-hosted Bitrix24, see [Update Bitrix24 On-Premise](https://helpdesk.bitrix24.com/open/24875124/).

After the update, check method availability again using [methods](../../../api-reference/common/system/methods.md) or [method.get](../../../api-reference/common/system/method-get.md), and event availability using [events](../../../api-reference/events/events.md).

If the method or event is still unavailable after the update, check whether the corresponding module is installed and whether the application has the required [access permissions](../../../api-reference/scopes/permissions.md).

## Continue Learning

- [{#T}](index.md)
- [{#T}](../../../api-reference/common/system/methods.md)
- [{#T}](../../../api-reference/events/events.md)
