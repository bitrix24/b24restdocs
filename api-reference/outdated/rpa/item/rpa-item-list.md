# Retrieve an Array of Process Elements rpa.item.list

> Scope: [`rpa`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

{% note warning "DEPRECATED" %}

The development of this method has been halted. Please use [Smart Processes](../../../crm/universal/user-defined-object-types/index.md) as an alternative to this functionality.

{% endnote %}

This method retrieves a list of process elements with the identifier `typeId`.

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

Keys for filtering by custom fields should be in `UPPER_CASE`, while others should be in `camelCase`. Examples of filters are provided [below](#filters) ||
|| **start** 
[`number`](../../../data-types.md)  | Offset for pagination. ||
|#

## Response Handling

HTTP Status: **200**

The response will contain only the main fields of the elements, without data about tasks and users of the elements:

```json
{
    "items": [
        {},
        {}
    ]
}
```

## Filter Examples {#filters}

1. Find elements that have uncompleted tasks for the current user

    ```json
    {
        "filter": {
            "tasks": "has_tasks"
        }
    }
    ```

    To find elements that do not have tasks assigned to the user, pass the value `no_tasks`.

2. Find elements updated by the user with the identifier `4`

    ```json
    {
        "filter": {
            "=updatedBy": "4"
        }
    }
    ```

3. Find elements updated or moved by the user with the identifier `4`

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

4. Find elements where the custom field with the code `UF_RPA_1_STRING` is filled

    ```json
    {
        "filter": {
            "!=UF_RPA_1_STRING": ""
        }
    }
    ```

5. Find elements that were created, modified, and moved between March 19 and March 22

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

6. Find elements that were either created, modified, or moved between March 19 and March 22

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