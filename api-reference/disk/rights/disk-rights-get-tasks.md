# Get a list of available access levels disk.rights.getTasks

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- parameter requirements are not specified
- examples are missing (there should be three examples - curl, js, php)
- success response is missing
- error response is missing

{% endnote %}

{% endif %}

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon

{% endnote %}

> Scope: [`disk`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `disk.rights.getTasks` allows you to retrieve a list of access levels that can be used for assigning permissions. It returns the available access levels.

## Parameters

#|
||  **Parameter** / **Type**| **Description** ||
|| **ID**
[`unknown`](../../data-types.md) | Identifier of the access level. ||
|| **NAME**
[`unknown`](../../data-types.md) | Symbolic code. ||
|| **TITLE**
[`unknown`](../../data-types.md) | Title. ||
|| **START** | The ordinal number of the list item from which to return the next items when calling the current method. Details in the article [{#T}](../../how-to-call-rest-api/list-methods-pecularities.md) ||
|#

## Example

{% list tabs %}

- JS

    ```js
    BX24.callMethod(
        "disk.rights.getTasks",
        {},
        function (result) {
            if (result.error())
                console.error(result.error());
            else
                console.dir(result.data());
        }
    )
    ```

{% endlist %}

{% include [Footnote about examples](../../../_includes/examples.md) %}

## Success response

> 200 OK

```json
{
    "result": [
        {
            "ID": "42",
            "NAME": "disk_access_full",
            "TITLE": "Full access"
        },
        {
            "ID": "40",
            "NAME": "disk_access_edit",
            "TITLE": "Editing"
        },
        {
            "ID": "38",
            "NAME": "disk_access_read",
            "TITLE": "Reading"
        }
    ]
}
```