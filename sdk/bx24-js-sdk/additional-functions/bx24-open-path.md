# Open Path in the BX24.openPath Slider

The method `BX24.openPath` opens the specified path within Bitrix24 in a slider.

```js
void BX24.openPath(String path[, Function callback])
```

{% note warning "" %}

For security reasons, this method does not work in the mobile application.

{% endnote %}

## Parameters

{% include [Required parameters note](../../../_includes/required.md) %}

#| 
|| **Name**
`type` | **Description** ||
|| **path*** 
`string` | The path within Bitrix24. The following prefixes are supported:

- `/crm/deal/` — CRM deal detail forms and sections
- `/crm/lead/` — CRM lead detail forms and sections
- `/crm/contact/` — CRM contact detail forms and sections
- `/crm/company/` — CRM company detail forms and sections
- `/crm/type/` — CRM Smart Processes
- `/marketplace/` — Marketplace pages
- `/company/personal/user/{ID}/` — User profile
- `/workgroups/group/{ID}/` — Workgroup or project ||
|| **callback**
`function` | Callback function. Invoked when there is an error opening the path or when the slider is closed ||
|#

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