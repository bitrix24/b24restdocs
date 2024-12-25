# Get a list of all resources calendar.resource.list

> Scope: [`calendar`](../scopes/permissions.md)
>
> Who can execute the method: any user

The method `calendar.resource.list` returns a list (array) of all resources.

Without parameters.

## Example

{% list tabs %}

- JS

    ```js
    BX24.callMethod('calendar.resource.list')
    ```

{% endlist %}

{% include [Footnote on examples](../../_includes/examples.md) %}

## Response Handling

HTTP status: **200**

```json
{
  "result": [
    {
      "ID": "198",
      "NAME": "Resource name",
      "CREATED_BY": "1"
    },
    {
      "ID": "199",
      ...
    }
  ],
  "time": {
    "start": 1733318565.183275,
    "finish": 1733318565.695058,
    "duration": 0.5117831230163574,
    "processing": 0.29406094551086426,
    "date_start": "2024-12-04T13:22:45+00:00",
    "date_finish": "2024-12-04T13:22:45+00:00"
  }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`array`](../data-types.md) | Array of resources ||
|| **ID**
[`string`](../data-types.md) | Resource identifier ||
|| **NAME**
[`string`](../data-types.md) | Resource name ||
|| **CREATED_BY**
[`string`](../data-types.md) | Identifier of the resource creator ||
|#

## Error Handling

HTTP status: **400**

```json
{
  "error": "",
  "error_description": "Access denied"
}
```

{% include notitle [error handling](../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Error Message** | **Description** ||
|| Empty string | Access denied | Access to the method is prohibited for external users ||
|#

{% include [system errors](../../_includes/system-errors.md) %}