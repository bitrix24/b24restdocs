# Widgets Embedded in the Interface of Custom CRM Object Types

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Widgets embed the application interface directly into Bitrix24: your own tab in an item card, your own button above the timeline. The general mechanism is described in the [{#T}](../../widgets/index.md) section.

Custom CRM object types — smart processes — differ from deals and leads in that they are created on an already configured Bitrix24. That is why placements for a smart process appear only after the type has been added, and the placement code contains the numeric identifier of that type.

For example, if a smart process named Vendors with type identifier `131` has been added, a tab in the item detail card is available for it under the code `CRM_DYNAMIC_131_DETAIL_TAB`, and a button above the timeline under the code `CRM_DYNAMIC_131_DETAIL_ACTIVITY`.

## How to Embed a Widget into a Smart Process

1. Retrieve the type identifier. The placement code uses the `entityTypeId` of the smart process: it is returned by the [crm.type.list](./user-defined-object-types/crm-type-list.md) method in the `entityTypeId` key or by the [crm.enum.ownertype](../auxiliary/enum/crm-enum-owner-type.md) method in the `ID` key. Do not confuse `entityTypeId` with the `id` of the smart process — these are different values, and one is not derived from the other. The other identifiers of a smart process are listed in the [Smart Process Identifiers](./user-defined-object-types/index.md#id) section.

2. Build the placement code. Placement codes for smart processes follow the pattern `CRM_DYNAMIC_XXX_DETAIL_TAB`, where `XXX` is replaced with the `entityTypeId`. For type `131`, the tab in the item card is `CRM_DYNAMIC_131_DETAIL_TAB`, and the button above the timeline is `CRM_DYNAMIC_131_DETAIL_ACTIVITY`. The full list of codes available to smart processes is given in the [{#T}](./user-defined-object-types/index.md) section.

3. Register the handler with the [placement.bind](../../widgets/placement-bind.md) method: pass the placement code in the `PLACEMENT` parameter and the address of your handler in `HANDLER`. The method is available only to an administrator and requires the application context: a placement cannot be registered with a webhook.

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"PLACEMENT":"CRM_DYNAMIC_131_DETAIL_TAB","HANDLER":"https://myapp.com/handler/","TITLE":"Vendors","auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/placement.bind
    ```

4. Complete the [application installation](../../../settings/app-installation/installation-finish.md). Until then, the widget does not appear in the interface.

## Other Placements

> Scope: [`placement, crm`](../../scopes/permissions.md)

A tab in the card and a button above the timeline are not the only placements. Smart processes support others as well: items in the context menu and in the menu above the item list, an item in the menu of the card top button, a button in the document generator, and an item in the Automation rules designer menu. What each placement does, what the handler receives when it is called, and which errors are possible during registration are covered in the [{#T}](../../widgets/crm/index.md) overview.

## Continue Your Exploration

- [{#T}](../../widgets/crm/index.md)
- [{#T}](../../widgets/placement-bind.md)
- [{#T}](../../widgets/placement-get.md)
- [{#T}](../../widgets/placement-unbind.md)
- [{#T}](./user-defined-object-types/index.md)
- [{#T}](../../widgets/index.md)