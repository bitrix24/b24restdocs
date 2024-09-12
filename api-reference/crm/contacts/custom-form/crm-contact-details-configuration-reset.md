# Reset Contact Card Parameters crm.contact.details.configuration.reset

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: permission checks when executing the method depend on the provided data:
>   - Any user has the right to reset their personal settings.
>   - A user can reset shared and others' settings only if they are an administrator.

The method `crm.contact.details.configuration.reset` resets the settings of contact cards. The method removes the personal settings of the specified user or the shared settings defined for all users.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **scope**
[`string`](../../../data-types.md) | The scope of the settings.

Possible values:
- **P** - personal settings,
- **C** - shared settings.

Default - `P`
||
|| **userId**
[`user`](../../../data-types.md) | User identifier. If not specified, the current user is taken. Required only when resetting personal settings. ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

### Reset Shared Configuration

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
            'crm.contact.details.configuration.reset',
            {
                scope: "C",
            },
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

### Reset Personal Configuration

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
            'crm.contact.details.configuration.reset',
            {
                scope: "P",
                userId: 6,
            },
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
		"start": 1724682584.069094,
		"finish": 1724682584.38436,
		"duration": 0.31526613235473633,
		"processing": 0.025727033615112305,
		"date_start": "2024-08-26T16:29:44+02:00",
		"date_finish": "2024-08-26T16:29:44+02:00",
		"operating": 0
	}
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../../data-types.md) | Root element of the response. Returns `true` in case of successful settings reset. ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the execution time of the request. ||
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
|| `-`     | Access denied. | The user does not have administrative rights. ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

TODO