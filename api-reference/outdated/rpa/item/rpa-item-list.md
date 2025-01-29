# Get an Array of Process Items rpa.item.list

> Scope: [`rpa`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method retrieves a list of process items with the identifier `typeId`.

## Method Parameters

#|
|| **Name**
`type` | **Description** ||
|| **typeId** 
[`integer`](../../../data-types.md) | Identifier of the process ||
|| **order** 
[`number`](../../../data-types.md)  | List for sorting, where the key is the field and the value is `ASC` or `DESC` ||
|| **filter** 
[`number`](../../../data-types.md)  | List for filtering.

Keys for filtering by custom fields should be in `UPPER_CASE`, while others should be in `camelCase`. Examples of filters [below](#filters) ||
|| **start** 
[`number`](../../../data-types.md)  | Offset for pagination. ||
|#

## Response Handling

HTTP Status: **200**

The response will only include the main fields of the items, without data about tasks and users of the items:

```json
{
    "items": [
        {},
        {}
    ]
}
```

## Filter Examples {#filters}

1. Find items that have incomplete tasks for the current user

    ```json
    {
        "filter": {
            "tasks": "has_tasks"
        }
    }
    ```

    To find items that do not have any tasks for the user, you need to pass the value `no_tasks`.

2. Find items updated by the user with identifier `4`

    ```json
    {
        "filter": {
            "=updatedBy": "4"
        }
    }
    ```

3. Find items updated or moved by the user with identifier `4`

    ```json
    {
        "filter": {
            "logic": "or",
            "0": {
                "=updatedBy": "4"
            },
            "1": {
                "=movedBy": "4"
            }
        }
    }
    ```

4. Find items that have a filled custom field with code `UF_RPA_1_STRING`

    ```json
    {
        "filter": {
            "!=UF_RPA_1_STRING": "",
        }
    }
    ```

5. Find items that were created, modified, and moved between March 19 and March 22

    ```json
    {
        "filter": {
            ">createdTime":"2020-03-19T02:00:00+02:00",
            ">movedTime":"2020-03-19T02:00:00+02:00",
            ">updatedTime":"2020-03-19T02:00:00+02:00",
            "<createdTime":"2020-03-22T02:00:00+02:00",
            "<movedTime":"2020-03-22T02:00:00+02:00",
            "<updatedTime":"2020-03-22T02:00:00+02:00"
        }
    }
    ```

6. Find items that were either created, modified, or moved between March 19 and March 22

    ```json
    {
        "filter": {
            "logic":"OR",
            "0":{
                ">createdTime":"2020-03-19T02:00:00+02:00",
                "<createdTime":"2020-03-22T02:00:00+02:00"
            },
            "1":{
                ">movedTime":"2020-03-19T02:00:00+02:00",
                "<movedTime":"2020-03-22T02:00:00+02:00"
            },
            "2":{
                ">updatedTime":"2020-03-19T02:00:00+02:00",
                "<updatedTime":"2020-03-22T02:00:00+02:00"
            }
        }
    }
    ```

## Continue Learning 

- [{#T}](./index.md)
- [{#T}](./rpa-item-add.md)
- [{#T}](./rpa-item-update.md)
- [{#T}](./rpa-item-get.md)
- [{#T}](./rpa-item-get-tasks.md)
- [{#T}](./rpa-item-delete.md)