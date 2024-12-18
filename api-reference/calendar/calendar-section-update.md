# Update Calendar calendar.section.update

> Scope: [`calendar`](../scopes/permissions.md)
>
> Who can execute the method: any user

The method `calendar.section.update` updates the calendar. Here and further, section will be referred to as "calendar".

## Method Parameters

{% include [Note on required parameters](../../_includes/required.md) %}

#|
|| **Parameter** | **Description** ||
|| **type***  
[`string`](../data-types.md) | Type of calendar: 
- user; 
- group. ||
|| **ownerId***  
[`integer`](../data-types.md) | Identifier of the calendar owner. ||
|| **id***  
[`string`](../data-types.md) | Identifier of the calendar. ||
|| **name**  
[`string`](../data-types.md) | Name of the calendar. ||
|| **description**  
[`string`](../data-types.md) | Description of the calendar. ||
|| **color**  
[`string`](../data-types.md) | Color of the calendar. ||
|| **text_color**  
[`string`](../data-types.md) | Text color in the calendar. ||
|| **export**  
[`object`](../data-types.md) | List of parameters: 
- ALLOW [`boolean`](../data-types.md) - allow calendar export; 
- SET [`string`](../data-types.md) - sets the period for export ('all', '3_9', '6_12'):
  - all - for the entire period;
  - 3_9 - 3 months before and 9 after;
  - 6_12 - 6 months before and 12 after.
||
|#

## Code Examples

{% include [Note on examples](../../_includes/examples.md) %}

{% list tabs %}

- JS

    ```js
    BX24.callMethod(
        'calendar.section.update',
        {
            id: 325,
            type: 'user',
            ownerId: 2,
            name: 'Changed Section Name',
            description: 'New description for section',
            color: '#9cbeAA',
            text_color: '#283099',
            export: [{ALLOW: false}]
        }
    );
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
  "result": 190,
  "time": {
    "start": 1733812564.64201,
    "finish": 1733812565.71673,
    "duration": 1.0747201442718506,
    "processing": 0.05963897705078125,
    "date_start": "2024-12-08T06:36:04+00:00",
    "date_finish": "2024-12-08T06:36:05+00:00"
  }
}
```

### Returned Data

#|
|| **Name**  
`type` | **Description** ||
|| **result**  
[`integer`](../data-types.md) | Identifier of the created calendar ||
|#

## Error Handling

HTTP status: **400**

```json
{
  "error": "",
  "error_description": "The required parameter \"type\" for the method \"calendar.section.update\" is not set"
}
```

{% include notitle [error handling](../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Error Message** | **Description** ||
|| Empty string | The required parameter "type" for the method "calendar.section.update" is not set | The required parameter `type` is not provided ||
|| Empty string | The required parameter "ownerId" for the method "calendar.section.update" is not set | The required parameter `ownerId` is not provided and the parameter `type` is not equal to 'user' ||
|| Empty string | Section id is not set | The required parameter `id` is not provided ||
|| Empty string | Invalid value for parameter "name" | Incorrect data format in the `name` field ||
|| Empty string | Invalid value for parameter "description" | Incorrect data format in the `description` field ||
|| Empty string | Access denied | The calendar with the specified `id` does not exist or there are no rights to edit the calendar ||
|| Empty string | An error occurred while changing the section | Another error ||
|#

{% include [system errors](../../_includes/system-errors.md) %}