# Add a New Calendar calendar.section.add

> Scope: [`calendar`](../scopes/permissions.md)
>
> Who can execute the method: any user

The method `calendar.section.add` adds a new calendar. Here and further, section will be referred to as "calendar".

{% note info %}

Currently, the method adds a new calendar only for the user executing the method calendar.section.add. This limitation will be lifted in the future.

{% endnote %}

## Method Parameters

{% include [Note on required parameters](../../_includes/required.md) %}

#|
|| **Parameter** | **Description** ||
|| **type***
[`string`](../data-types.md) | Calendar type: 
- user; 
- group. ||
|| **ownerId***
[`integer`](../data-types.md) | Identifier of the calendar owner. If not provided, it will be automatically set to the current user for type == 'user' ||
|| **name***
[`string`](../data-types.md) | Calendar name ||
|| **description**
[`string`](../data-types.md) | Calendar description ||
|| **color**
[`string`](../data-types.md) | Calendar color ||
|| **text_color**
[`string`](../data-types.md) | Text color in the calendar ||
|| **export**
[`object`](../data-types.md) | List of calendar export parameters: 
- ALLOW [`boolean`](../data-types.md) - allow calendar export; 
- SET [`string`](../data-types.md) - sets the period for export ('all', '3_9', '6_12')
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
        'calendar.section.add',
        {
            type: 'user',
            ownerId: 2,
            name: 'New Section',
            description: 'Description for section',
            color: '#9cbeee',
            text_color: '#283000',
            export: {ALLOW: false, SET: '3_9'}
        }
    );
    ```

{% endlist %}

{% include [Note on examples](../../_includes/examples.md) %}

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
  "error_description": "The required parameter \"type\" for the method \"calendar.section.add\" is not set"
}
```

{% include notitle [error handling](../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Error Message** | **Description** ||
|| Empty string | The required parameter "type" for the method "calendar.section.add" is not set | The required parameter `type` is not provided ||
|| Empty string | The required parameter "ownerId" for the method "calendar.section.add" is not set | The required parameter `ownerId` is not provided and the `type` parameter is not equal to 'user' ||
|| Empty string | Invalid value for the "name" parameter | Incorrect data format in the `name` field ||
|| Empty string | Invalid value for the "description" parameter | Incorrect data format in the `description` field ||
|| Empty string | Access denied | No permission to create a calendar with the provided `type` ||
|| Empty string | An error occurred while creating the section | Another error ||
|#

{% include [system errors](../../_includes/system-errors.md) %}