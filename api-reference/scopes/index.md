# Access Permissions for Methods and Scopes

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Scopes define which groups of methods the application will have access to. A scope is represented by a short code, such as `crm`, `telephony`, or `user`. When you add a mass-market solution in the partner's account or an on-premise solution in Bitrix24, specify the scopes that the application needs to function.

> Quick navigation: [overview of pages](#pages)
>
> User documentation: [Create webhooks and apps in Bitrix24](https://helpdesk.bitrix24.com/open/21133100/)

## How to Determine Which Scopes Are Needed for the Application

Start not with a list of all scopes, but with the application's scenario. Identify which methods the application will call directly and check the `Scope` block on the pages of those methods.

For example, the application connects external telephony and directly calls the methods [telephony.externalCall.register](../telephony/telephony-external-call-register.md) and [telephony.externalCall.finish](../telephony/telephony-external-call-finish.md). Since the application calls telephony methods, the `telephony` scope is required in its settings.

The `crm` scope is not needed for these actions, even though telephony methods may interact with CRM within Bitrix24: [telephony.externalCall.register](../telephony/telephony-external-call-register.md) can automatically create a CRM entity, such as a lead, while [telephony.externalCall.finish](../telephony/telephony-external-call-finish.md) saves the call in a CRM activity.

The `crm` scope is only necessary if the application itself sends requests to CRM methods, such as [crm.lead.add](../crm/leads/crm-lead-add.md) or [crm.activity.add](../crm/timeline/activities/activity-base/crm-activity-add.md).

The order of selecting scopes:

1. Find the method pages needed for the scenario.
2. Check the code in the `Scope` block at the top of each method page.
3. Add only the scopes of the methods that the application calls directly to the application settings.
4. Review the line `Who can execute the method`.
5. If the method requires administrator confirmation, use the scenario from the article [Calling Methods with Confirmation](./confirmation.md).

## How Scopes Differ from User Permissions

Scopes and user permissions are checked separately.

A scope indicates whether the application is allowed to access a group of methods. User permissions indicate whether a specific user can perform an action in Bitrix24.

At the beginning of each method's page, there is a block:

> Scope: `crm`
>
> Who can execute the method: any user

If the line `Who can execute the method` specifies administrative rights or a special tool permission, a single scope is not sufficient. The method must be executed on behalf of a user who has the necessary permissions.

## Overview of Pages {#pages}

#| 
|| **Page** | **What it helps to do** ||
|| [Available Scopes in Bitrix24](./permissions.md) | Find the scope code and the associated Bitrix24 tool ||
|| [Calling Methods with Confirmation](./confirmation.md) | Prepare the application for methods that require administrator permission ||
|#