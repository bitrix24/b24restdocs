# Update Status "Delivered" imconnector.send.status.delivery

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter type is not specified
- parameter requirement is not specified
- examples are missing
- responses in case of success and error are not specified

{% endnote %}

{% endif %}

> Scope: [`imopenlines`](../../scopes/permissions.md)
>
> Who can execute the method: any user

This method confirms the delivery of a message from OL to an external system.

## Parameters

#|
|| **Parameter** | **Description** | **Version** ||
|| **CONNECTOR**
[`unknown`](../../data-types.md) | Connector ID (which was specified during the handler registration). | ||
|| **LINE**
[`unknown`](../../data-types.md) | Open line ID. | ||
|| **MESSAGES**
[`unknown`](../../data-types.md) | An array of messages, where messages are described by an array of the following format: 

```
    array(
        array(
            'im',//The 'im' element from the incoming OL message is forwarded
            'message' => array(
                'id'//Array of IDs in the external system. It is an array, not a single 
                    //value, even if there is only one ID.
            ),
            'chat' => array(
                'id'//Chat ID in the external system.
            ),
        ),
        array(...),
    );
```
| ||
|#

{% include [Parameter Notes](../../../_includes/required.md) %}