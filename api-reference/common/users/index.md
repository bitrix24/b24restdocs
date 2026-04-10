# User Information: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

The methods in this group work exclusively with the current user: they retrieve basic profile data, check access permissions, and manage application settings.

Information about other users can be obtained through the [Users](../../user/index.md) group of methods.

> Quick Navigation: [all methods](#all-methods)

## How to Retrieve Current User Data

The [profile](./profile.md) method retrieves basic data about the current user, such as `ID`, first name, last name, admin status, and time zone.

## How to Check Current User Access

The [user.access](./user-access.md) method checks whether the current user has at least one of the provided access codes, such as `G2` or `AU`.

The [user.admin](./user-admin.md) method checks if the current user has the rights to manage application settings.

## Access Codes in user.access   {#kody-dostupa-v-useraccess}

The [user.access](./user-access.md) method accepts an array of access codes in the `ACCESS` parameter.

#| 
|| **Code** | **Meaning** | **Example** ||
|| `U<id>` | User | `U1` ||
|| `G<id>` | User Group | `G2` ||
|| `AU` | All authorized users | `AU` ||
|#

If the access code is already known, use [user.access](./user-access.md). To obtain the name of the access code, use [access.name](../system/access-name.md).

## Overview of Methods      {#all-methods}

> Scope: [`basic`](../../scopes/permissions.md)
>
> Who can execute the method: any user

#| 
|| **Method** | **Description** ||
|| [user.admin](./user-admin.md) | Checks if the current user can manage application settings ||
|| [user.access](./user-access.md) | Checks if the current user has at least one of the specified access codes (`ACCESS`) ||
|| [profile](./profile.md) | Retrieves information about the current user ||
|#