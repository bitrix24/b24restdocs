# Set Common Detail Form for All Users crm.contact.details.configuration.forceCommonScopeForAll

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
> 
> Who can execute the method: Administrator

The method `crm.contact.details.configuration.forceCommonScopeForAll` allows you to forcibly set a common contact detail form for all users.

## Method Parameters

No parameters.

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

Set a common detail form for contacts for all users.

{% list tabs %}

- cURL (Webhook)

    ```bash
    todo
    ```

- cURL (OAuth)

    ```bash
    todo
    ```

- JS

    ```js
    BX24.callMethod(
        'crm.contact.details.configuration.forceCommonScopeForAll',
        {},
        (result) => {
            result.error()
                ? console.error(result.error())
                : console.info(result.data())
            ;
        },
    );
    ```

- PHP

    ```php
    todo
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
	"result": true,
	"time": {
		"start": 1724671860.18392,
		"finish": 1724671860.843895,
		"duration": 0.6599750518798828,
		"processing": 0.09691596031188965,
		"date_start": "2024-08-26T13:31:00+02:00",
		"date_finish": "2024-08-26T13:31:00+02:00",
		"operating": 0
	}
}
```

### Returned Values

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../../data-types.md) | Root element of the response. Returns `true` on success ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
  "error": "",
  "error_description": "Access denied."
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description**   | **Value** ||
|| `-`     | Access denied. | The user does not have administrative rights ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

TODO