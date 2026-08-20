# Universal List Sections: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Sections help organize data and simplify navigation within lists. They group records by categories or levels of nesting.

> Quick navigation: [all methods](#all-methods)
>
> User documentation: [List View Settings](https://helpdesk.bitrix24.com/open/18128266/)

## Getting Started with Sections

1. Retrieve list parameters using the [lists.get](../lists/lists-get.md) method.
2. Create a section using the [lists.section.add](./lists-section-add.md) method.
3. Retrieve, update, and delete sections using the [lists.section.get](./lists-section-get.md), [lists.section.update](./lists-section-update.md), and [lists.section.delete](./lists-section-delete.md) methods.

## Relationships with Other Objects

**Universal lists.** Each section belongs to a specific list. To add a section to a list, you need to specify the parameters `IBLOCK_TYPE_ID`, `IBLOCK_ID`, and `IBLOCK_CODE`. Values can be obtained using the [lists.get](../lists/lists-get.md) method.

## Overview of Methods {#all-methods}

> Scope: [`lists`](../../scopes/permissions.md)
>
> Who can execute the method: depending on the method

#|
|| **Method** | **Description** ||
|| [lists.section.add](./lists-section-add.md) | Creates a list section ||
|| [lists.section.update](./lists-section-update.md) | Updates a list section ||
|| [lists.section.get](./lists-section-get.md) | Returns a section or a list of sections ||
|| [lists.section.delete](./lists-section-delete.md) | Deletes a list section ||
|#
