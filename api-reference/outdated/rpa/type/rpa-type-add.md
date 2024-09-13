# Create a New Process rpa.type.add

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- parameter types are not specified
- examples are missing
- response in case of error is absent

{% endnote %}

{% endif %}

> Scope: [`rpa`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `rpa.type.add` will create a new process.

#|
|| **Parameter** / **Type** | **Description** ||
|| **fields**
[`array`](../../../data-types.md) | A list of process fields. The list of possible fields is described below.||
|#

## Parameters fields

#|
|| **Parameter** | **Description** ||
|| **title**^*^ | The name of the process. ||
|| **image** | The image of the process from the list. ||
|| **settings** | A list with an arbitrary set of process settings. ||
|| **permissions** | An array with access permissions for this process. ||
|#

{% include [Notes on parameters](../../../../_includes/required.md) %}

{% note warning %}

Automatic scenarios (creating stages, automation rules, and default fields) will not be triggered when creating a process via `rest`.

{% endnote %}

{% note warning %}

Important! The request must include access permissions for modifying the process.

{% endnote %}

## Example

This request will create a new process named "My Process". All users will be able to create entities of this process. Only the user with ID 1 will be able to change the settings of this process.

```json
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
            }
        ]
    }
}
```

{% include [Notes on examples](../../../../_includes/examples.md) %}

## Response on Success

 It will return data in the response similar to the response for the request [rpa.type.get](./rpa-type-get.md).