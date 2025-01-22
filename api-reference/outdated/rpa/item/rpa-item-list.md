# Get an Array of Process Elements rpa.item.list

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- required parameters are not specified
- examples are missing
- response in case of error is absent

{% endnote %}

{% endif %}

> Scope: [`rpa`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `rpa.item.list` will return an array of process elements with the identifier typeId.

#|
|| **Name**
`type` | **Description** ||
|| **typeId** 
[`number`](../../../data-types.md) | Identifier of the process. ||
|| **order**  |  List for sorting, where the key is the field, and the value is ASC or DESC. ||
|| **filter**  | List for filtering. Keys for filtering by custom fields should be in UPPER_CASE, others in camelCase. Examples of filters are below. ||
|| **start**  | Offset for pagination. ||
|#

## Response on Success

> 200 OK

The response will contain only the main fields of the elements, without data about tasks and users of the elements:

```json
{
    "items": [
        {},
        {}
    ]
}
```

## Filter Examples

1. **Find elements that have uncompleted tasks for the current user**

    ```json
    {
        "filter": {
            "tasks": "has_tasks"
        }
    }
    ```

    To find elements that have no tasks for the user, you need to pass the value `no_tasks`.

2. **Find elements updated by the user with identifier 4**

    ```json
    {
        "filter": {
            "=updatedBy": "4"
        }
    }
    ```

3. **Find elements updated or moved by the user with identifier 4**

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

4. **Find elements that have a filled custom field with code `UF_RPA_1_STRING`**

    ```json
    {
        "filter": {
            "!=UF_RPA_1_STRING": "",
        }
    }
    ```

5. **Find elements that were created, modified, and moved between March 19 and March 22**

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

6. **Find elements that were either created, modified, or moved between March 19 and March 22**

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

{% include [Examples Note](../../../../_includes/examples.md) %}