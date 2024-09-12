# Update Deal crm.deal.update

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- parameter requirements are not specified
- examples (in other languages) are missing
- success response is missing
- error response is missing

{% endnote %}

{% endif %}

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `crm.deal.update` updates an existing deal.

#|
|| **Parameter** | **Description** ||
|| **id** | Deal identifier. ||
|| **fields** | [Set of fields](./crm-deal-add.md), an array in the form `array("field_to_update"=>"value"[, ...])`, where "field_to_update" can take values returned by the method [crm.deal.fields](./crm-deal-fields.md). 

{% note info %} 

To find out the required format of the fields, execute the method [crm.deal.fields](./crm-deal-fields.md) and check the format of the returned values for those fields. 

{% endnote %} ||
|| **params** | Set of parameters. `REGISTER_SONET_EVENT` - register the event of the deal change in the live feed. A notification will also be sent to the person responsible for the deal. ||
|#

## Example

```js
var id = prompt("Enter ID");
BX24.callMethod(
    "crm.deal.update",
    {
        id: id,
        fields:
        {
            "STAGE_ID": "NEGOTIATION",
            "PROBABILITY": 70
        },
        params: { "REGISTER_SONET_EVENT": "Y" }
    },
    function(result)
    {
        if(result.error())
            console.error(result.error());
        else
        {
            console.info(result.data());
        }
    }
);
```

How to upload a file to a deal via webhook (PHP)

```php
$batchUpdate = array (
    'update_contact' => 'crm.deal.update?'.http_build_query(
        array(
            'id'=> $dealId,
            'fields'=> array(
                'fileData'=>array(
                    $files['files']['name'], base64_encode(file_get_contents($files['files']['tmp_name'])),
                )
            ),
        )
    )
)

$resultUpdate = executeHook(array('cmd' => $batchUpdate)); // execute the hook
```

{% include [Note on examples](../../../_includes/examples.md) %}

## Method Explanation

To manage deal contacts, it is recommended to use the multiple field CONTACT_IDS:

Example:

```js
BX24.callMethod("crm.deal.update", { id: 1, fields: { "CONTACT_IDS": [ 1, 2, 3 ] } });
```

As a result, the deal will be linked to the three specified contacts.

The field `CONTACT_ID` is deprecated and is supported for backward compatibility.

Example:

```js
BX24.callMethod("crm.deal.update", { id: 1, fields: { "CONTACT_ID": 4 } });
```

As a result of this call, the deal will be linked to the specified contact. 

{% note warning %}

Please note that existing links to contacts will not be removed. That is, if the deal was previously linked to contacts 1, 2, and 3, it will now be linked to contacts 1, 2, 3, and 4.

{% endnote %}


{% note tip "Related methods and topics" %}

[{#T}](./recurring-deals/crm-deal-recurring-update.md)

{% endnote %}