# Open Path in the BX24.openPath Slider

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

```js
BX24.openPath(path: string, callback?: callable): void;
```

The `BX24.openPath` method opens a Bitrix24 page in a slider over the application, for example, a deal detail form or an employee profile. When the user closes the slider, they return to the application.

The method works only inside the application frame in Bitrix24. Call it after the library is initialized, in the [BX24.init](../system-functions/bx24-init.md) handler. The method requires no scope of its own: it controls the interface and does not call the REST API.

{% note warning "" %}

The method does not work on phones and tablets — neither in the mobile application nor in a mobile browser. The slider does not open, and `callback` receives the `METHOD_NOT_SUPPORTED_ON_DEVICE` error.

{% endnote %}

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#| 
|| **Name**
`type` | **Description** ||
|| **path*** 
[`string`](../../../api-reference/data-types.md) | The path to a page in the same Bitrix24 instance where the application is open. The path starts with `/`. The method does not open a full address with a protocol and domain, even if it is an address of that same Bitrix24 instance.

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
[`callable`](../../../api-reference/data-types.md) | Callback function. It is called once: when the user closes the slider, when the path fails validation, or when the application is open on a phone [(detailed description)](#callback) ||
|#

To retrieve CRM object identifiers, use the list and creation methods, for example [crm.item.list](../../../api-reference/crm/universal/crm-item-list.md) and [crm.item.add](../../../api-reference/crm/universal/crm-item-add.md). To retrieve an employee identifier, use [user.get](../../../api-reference/user/user-get.md), and for a workgroup identifier use [sonet_group.get](../../../api-reference/sonet-group/sonet-group-get.md).

Bitrix24 opens the page with additional parameters in the address: `from=rest_placement&from_app=<application code>`. The application code is its `client_id`, for example `local.6ab58ce578dd87.16527213`. As a result, the address of the opened page differs from the path you passed — keep this in mind if you compare addresses.

## Code Example

{% include [Note on examples](../../../_includes/examples.md) %}

```js
BX24.init(function () {
    BX24.openPath('/crm/deal/details/5/', function (result) {
        if (result.result === 'error') {
            console.log('Failed to open the page:', result.errorCode);
            return;
        }

        console.log('The user closed the slider');
    });
});
```

## Response Handling {#callback}

The method does not return data (`void`). When the user closes the slider, Bitrix24 calls `callback` and passes an object:

```json
{
    "result": "close"
}
```

If the path fails validation or the application is open on a phone, `callback` is called immediately, without a slider:

```json
{
    "result": "error",
    "errorCode": "PATH_NOT_AVAILABLE"
}
```

### Returned Data

#| 
|| **Name**
`type` | **Description** ||
|| **result**
[`string`](../../../api-reference/data-types.md) | Outcome: `close` — the user closed the slider, `error` — the slider did not open ||
|| **errorCode**
[`string`](../../../api-reference/data-types.md) | Error code. Present only when `result: "error"` ||
|#

## Error Handling

#|
|| **Code** | **When It Occurs** | **What to Do** ||
|| `PATH_NOT_AVAILABLE` | The path fails validation: it is empty, does not start with `/` (like the full address `https://example.com/`), leads to another site, or contains a `%` sign that is not followed by a character code, like `/crm/%` | Pass a path relative to the Bitrix24 root, for example `/crm/deal/details/5/` ||
|| `METHOD_NOT_SUPPORTED_ON_DEVICE` | The application is open on a phone or tablet | Show the user where to find the page, or suggest opening it on a computer ||
|#

{% note warning "" %}

The method does not check whether the page exists. If you pass a path to a nonexistent page, the slider opens anyway, and after it is closed, `callback` receives `close`.

{% endnote %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./bx24-open-application.md)
- [{#T}](./bx24-close-application.md)