# Open Path in the BX24.openPath Slider

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

The method `BX24.openPath` opens the specified path within Bitrix24 in a slider.

```js
void BX24.openPath(String path[, Function callback])
```

{% note warning "" %}

For security reasons, this method does not work in the mobile application.

{% endnote %}

## Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#| 
|| **Name**
`type` | **Description** ||
|| **path*** 
`string` | The path within Bitrix24. It starts with `/` and leads to a page of the same Bitrix24 where the application is open. The method does not open external addresses or paths to other domains.

The numbers in the path are identifiers of specific objects. Substitute your own values for them:

- `/crm/deal/details/5/` — a deal detail form, where `5` is the deal identifier
- `/crm/lead/details/12/` — a lead detail form, where `12` is the lead identifier
- `/crm/contact/details/2/` — a contact detail form, where `2` is the contact identifier
- `/crm/company/details/7/` — a company detail form, where `7` is the company identifier
- `/crm/type/128/details/3/` — a smart process item detail form, where `128` is the smart process type identifier `entityTypeId`, and `3` is the identifier of the item itself
- `/company/personal/user/1/` — an employee profile, where `1` is the user identifier
- `/workgroups/group/4/` — a workgroup or project, where `4` is the group identifier
- `/marketplace/` — Marketplace, no identifier required ||
|| **callback**
`function` | Callback function. Invoked when there is an error opening the path or when the slider is closed ||
|#

To retrieve CRM object identifiers, use the list and creation methods, for example [crm.item.list](../../../api-reference/crm/universal/crm-item-list.md) and [crm.item.add](../../../api-reference/crm/universal/crm-item-add.md). To retrieve an employee identifier, use [user.get](../../../api-reference/user/user-get.md), and for a workgroup identifier use [sonet_group.get](../../../api-reference/sonet-group/sonet-group-get.md).

Before opening, the SDK automatically adds service parameters to the path: 
`from=rest_placement&from_app={appId}`.

## Code Example

```js
BX24.init(function () {
    BX24.openPath('/crm/deal/details/5/', function (result) {
        console.log(result);
    });
});
```

{% include [Examples note](../../../_includes/examples.md) %}

## Response Handling

The method does not return data (`void`).

If a `callback` is provided, it receives a result object.

### Callback Result

#| 
|| **Name**
`type` | **Description** ||
|| **result**
`string` | Execution status: `close` or `error` ||
|| **errorCode**
`string` | Error code. Passed only when `result: "error"`. Possible values: `PATH_NOT_AVAILABLE`, `METHOD_NOT_SUPPORTED_ON_DEVICE` ||
|#

## Continue Learning

- [{#T}](./bx24-open-application.md)
- [{#T}](./bx24-close-application.md)