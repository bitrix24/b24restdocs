# Add a new resource calendar.resource.add

> Scope: [`calendar`](../scopes/permissions.md)
>
> Who can execute the method: any user

The method `calendar.resource.add` adds a new resource and takes an array of parameters as input.

## Method Parameters

{% include [Note on required parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **name*** 
[`string`](../data-types.md) | The name of the resource. ||
|#

## Example

{% list tabs %}

- JS

    ```js
    BX24.callMethod(
        'calendar.resource.add',
        {
            name: 'My resource title'
        }
    );
    ```

{% endlist %}

{% include [Note on examples](../../_includes/examples.md) %}

## Response Handling

HTTP status: **200**

```json
{
  "result": 197,
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
[`integer`](../data-types.md) | Identifier of the created resource ||
|#

## Error Handling

HTTP status: **400**

```json
{
  "error": "",
  "error_description": "The required parameter \"name\" for the method \"calendar.resource.add\" is not set"
}
```
{% include notitle [error handling](../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Error Message** | **Description** ||
|| Empty string | The required parameter "name" for the method "calendar.resource.add" is not set | The required parameter `name` was not provided ||
|| Empty string | Access denied | The method is called by an external user or the user is prohibited from modifying resources ||
|| Empty string | An error occurred while creating the resource | Another error ||
|#

{% include [system errors](../../_includes/system-errors.md) %}