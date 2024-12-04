# Create Department department.add

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- no response in case of error
- no examples
  
{% endnote %}

{% endif %}

> Scope: [`department`](../scopes/permissions.md)
>
> Who can execute the method: a user with permissions to modify the structure

This method creates a department.

#|
|| **Parameter** | **Description** ||
|| **NAME^*^** | name of the department ||
|| **SORT** | sorting parameter for the department ||
|| **PARENT** | identifier of the parent department. ||
|| **UF_HEAD** | identifier of the department head ||
|#

{% include [Notes on parameters](../../_includes/required.md) %}

## Code Examples

{% list tabs %}

- JS

    ```js
    BX24.callMethod(
        'department.add',
        {
            "NAME": "Department",
            "PARENT": 155,
            "UF_HEAD": 1
        },
        function(result) {
            if (result.error()) {
                console.error(result.error());
            } else {
                console.info(result.data());
            }
        }
    );
    ```

{% endlist %}

## Request

```
https://my.bitrix24.com/rest/department.add.json?NAME=Department&PARENT=155&UF_HEAD=1&auth=1b2234a7412080205f4318e42c7298dc
```

## Response

> 200 OK

```json
{"result":222}
```