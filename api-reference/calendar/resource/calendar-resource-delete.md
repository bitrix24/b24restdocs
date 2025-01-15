# Delete Resource calendar.resource.delete

> Scope: [`calendar`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `calendar.resource.delete` removes a resource.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **resourceId*** | Resource identifier. ||
|#

## Example

{% list tabs %}

- JS

    ```js
    BX24.callMethod(
        'calendar.resource.delete',
        {
            resourceId: 521
        }
    );
    ```

{% endlist %}

{% include [Note on examples](../../../_includes/examples.md) %}

## Response Handling

HTTP status: **200**

```json
{
  "result": true,
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
[`boolean`](../../data-types.md) | Returns **true** if the deletion was successful. ||
|#

## Error Handling

HTTP status: **400**

```json
{
  "error": "",
  "error_description": "The required parameter \"resourceId\" for the method \"calendar.resource.delete\" is not set."
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Error Message** | **Description** ||
|| Empty string | The required parameter "resourceId" for the method "calendar.resource.delete" is not set. | The required parameter `resourceId` was not provided. ||
|| Empty string | Access denied | The method is called by an external user or the user is prohibited from modifying resources. ||
|| Empty string | An error occurred while deleting the section | Another error. ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}