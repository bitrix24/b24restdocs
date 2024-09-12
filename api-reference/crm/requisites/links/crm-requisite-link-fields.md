# Get Description of CRM Requisite Link Fields

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method returns a formal description of the link fields for requisites.

No parameters.

## Code Examples

{% include [Footnote on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.requisite.link.fields
    ```

- cURL (OAuth) 

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.requisite.link.fields
    ```

- JS

    ```js
    BX24.callMethod(
        "crm.requisite.link.fields",
        {},
        function(result)
        {
            if(result.error())
                console.error(result.error());
            else
                console.dir(result.data());
        }
    );
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.requisite.link.fields',
        []
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "ENTITY_TYPE_ID": {
            "type": "integer",
            "isRequired": true,
            "isReadOnly": false,
            "isImmutable": true,
            "isMultiple": false,
            "isDynamic": false,
            "title": "Object Type ID"
        },
        "ENTITY_ID": {
            "type": "integer",
            "isRequired": true,
            "isReadOnly": false,
            "isImmutable": true,
            "isMultiple": false,
            "isDynamic": false,
            "title": "Object ID"
        },
        "REQUISITE_ID": {
            "type": "integer",
            "isRequired": true,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "Link to Requisites"
        },
        "BANK_DETAIL_ID": {
            "type": "integer",
            "isRequired": true,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "Link to Bank Requisites"
        },
        "MC_REQUISITE_ID": {
            "type": "integer",
            "isRequired": true,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "Link to My Company's Requisites"
        },
        "MC_BANK_DETAIL_ID": {
            "type": "integer",
            "isRequired": true,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "Link to My Company's Bank Requisites"
        }
    },
    "time": {
        "start": 1718295307.777351,
        "finish": 1718295308.177586,
        "duration": 0.4002351760864258,
        "processing": 0.010699987411499023,
        "date_start": "2024-06-13T18:15:07+02:00",
        "date_finish": "2024-06-13T18:15:08+02:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../data-types.md) | An object in the format `{"field_1": "value_1", ... "field_N": "value_N"}`, where `field` is the field identifier and `value` is the object with [field attributes](#attributes) ||
|| **time**
[`time`](../../../data-types.md) | Information about the request execution time ||
|#

### Description of Requisite Link Fields to CRM Object

#|
|| **Name**
`type` | **Description** ||
|| **ENTITY_TYPE_ID**
[`integer`](../../../data-types.md) | Identifier of the object type to which the link pertains.

The following types can be used:
- deal (value `2`)
- old invoice (value `5`)
- estimate (value `7`)
- new invoice (value `31`)
- other dynamic objects (to get possible values, see the method [crm.type.list](../../universal/user-defined-object-types/crm-type-list.md)).

Object type identifiers can be obtained using the method [crm.enum.ownertype](../../auxiliary/enum/crm-enum-owner-type.md) 
||
|| **ENTITY_ID**
[`integer`](../../../data-types.md) | Identifier of the object to which the link pertains. 

Object identifiers can be obtained using the following methods: [crm.deal.list](../../deals/crm-deal-list.md), [crm.quote.list](../../quote/crm-quote-list.md), [crm.item.list](../../universal/crm-item-list.md) ||
|| **REQUISITE_ID**
[`integer`](../../../data-types.md) | Identifier of the client's requisite selected for the object. 

Requisite identifiers can be obtained using the method [crm.requisite.list](../universal/crm-requisite-list.md) ||
|| **BANK_DETAIL_ID**
[`integer`](../../../data-types.md) | Identifier of the client's bank requisite selected for the object.

Bank requisite identifiers can be obtained using the method [crm.requisite.bankdetail.list](../bank-detail/crm-requisite-bank-detail-list.md) ||
|| **MC_REQUISITE_ID**
[`integer`](../../../data-types.md) | Identifier of my company's requisite selected for the object. 

Requisite identifiers can be obtained using the method [crm.requisite.list](../universal/crm-requisite-list.md) ||
|| **MC_BANK_DETAIL_ID**
[`integer`](../../../data-types.md) | Identifier of my company's bank requisite selected for the object. 

Bank requisite identifiers can be obtained using the method [crm.requisite.bankdetail.list](../bank-detail/crm-requisite-bank-detail-list.md) ||
|#

### Description of Attributes {#attributes}

#|
|| **Name**
`type` | **Description** ||
|| **type**
[`string`](../../../data-types.md) | Field type ||
|| **isRequired**
[`boolean`](../../../data-types.md) | "Required" attribute. Possible values:
- true — yes
- false — no
||
|| **isReadOnly**
[`boolean`](../../../data-types.md) | "Read-only" attribute. Possible values:
- true — yes
- false — no
||
|| **isImmutable**
[`boolean`](../../../data-types.md) | "Immutable" attribute. Possible values:
- true — yes
- false — no
||
|| **isMultiple**
[`boolean`](../../../data-types.md) | "Multiple" attribute. Possible values:
- true — yes
- false — no
||
|| **isDynamic**
[`boolean`](../../../data-types.md) | "Custom" attribute. Possible values:
- true — yes
- false — no
||
|| **title**
[`string`](../../../data-types.md) | Field identifier ||
|#

## Error Handling

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-requisite-link-register.md)
- [{#T}](./crm-requisite-link-get.md)
- [{#T}](./crm-requisite-link-list.md)
- [{#T}](./crm-requisite-link-unregister.md)