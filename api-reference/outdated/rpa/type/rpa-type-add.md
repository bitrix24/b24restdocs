# Create process rpa.type.add

> Scope: [`rpa`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

This method creates a new process.

## Method Parameters

{% include [Footnote on parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields***
[`array`](../../../data-types.md) | A list of process fields. The list of possible fields is described [below](#fields) ||
|#

### Parameter fields {#fields}

{% include [Footnote on parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **title***
[`string`](../../../data-types.md) | The name of the process ||
|| **image**
[`string`](../../../data-types.md) | The image of the process from the list ||
|| **settings**
[`array`](../../../data-types.md) | A list with an arbitrary set of process settings ||
|| **permissions**
[`array`](../../../data-types.md) | A list of objects. Each object describes access permissions for this process ||
|#

{% note warning %}

- Automated scenarios, such as creating stages, Automation rules, and default fields, will not be triggered when creating a process via `rest`.
- The request must specify access permissions for modifying the process.

{% endnote %}

## Code Examples

Create a new process named "My Process". All users can create items for this process. Only the user with `id = 1` can change the settings of this process.

{% include [Footnote on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- JS

    ```js
    {
        "fields": {
            "title": "My Process",
            "image": "list",
            "permissions": [
                {
                    "accessCode": "UA",
                    "permission": "X",
                    "action": "ITEMS_CREATE"
                },
                {
                    "accessCode": "U1",
                    "permission": "X",
                    "action": "MODIFY"
                },
            ]
        }
    }
    ```

{% endlist %}

## Response Handling

The method will return data similar to the response of the method [rpa.type.get](./rpa-type-get.md).

## Continue Learning 

- [{#T}](./index.md)
- [{#T}](./rpa-type-update.md)
- [{#T}](./rpa-type-get.md)
- [{#T}](./rpa-type-list.md)
- [{#T}](./rpa-type-delete.md)