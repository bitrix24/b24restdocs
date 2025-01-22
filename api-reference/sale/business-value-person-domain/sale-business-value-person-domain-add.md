# Add Correspondence to a Natural or Legal Person sale.businessValuePersonDomain.add

> Scope: [`sale`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

The method `sale.businessValuePersonDomain.add` adds correspondence for the selected payer type to a natural or legal person. This is necessary for the operation of the business meanings mechanism.

## Method Parameters

{% include [Note on Required Parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields***
[`object`](../../data-types.md) | Field values for creating correspondence to a natural or legal person ||
|#

### Parameter fields

{% include [Note on Required Parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **personTypeId***
[`sale_person_type.id`](../data-types.md) | Identifier for the payer type. 

You can obtain the identifiers for payer types using the method [sale.persontype.list](../person-type/sale-person-type-list.md) ||
|| **domain***
[`string`](../../data-types.md) | Value corresponding to the payer type: natural or legal person.
- `I` — natural person
- `E` — legal person ||
|#

## Code Examples

{% include [Note on Examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```curl
    -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"personTypeId":3,"domain":"I"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sale.businessValuePersonDomain.add
    ```

- cURL (OAuth)

    ```curl
    -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"personTypeId":3,"domain":"I"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.businessValuePersonDomain.add
    ```

- JS

    ```js
    B24.callMethod(
        'sale.businessValuePersonDomain.add',
        {
            fields: {
                personTypeId: 3,
                domain: 'I'
            }
        },
        function(result)
        {
            if(result.error())
                console.error(result.error());
            else
                console.log(result.data());
        }
    );
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'sale.businessValuePersonDomain.add',
        [
            'fields' =>
            [
                'personTypeId' => 3,
                'domain' => 'I'
            ]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "businessValuePersonDomain": {
        "domain": "I",
        "personTypeId": 3
    },
    "time": {
        "start": 1712325642.686926,
        "finish": 1712325642.949075,
        "duration": 0.2621490955352783,
        "processing": 0.004400968551635742,
        "date_start": "2024-04-05T16:00:42+02:00",
        "date_finish": "2024-04-05T16:00:42+02:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Root element of the response ||
|| **businessValuePersonDomain**
[`sale_business_value_person_domain`](../data-types.md) | Object containing information about the added correspondence of the payer type to a natural or legal person ||
|| **time**
[`time`](../../data-types.md) | Information about the execution time of the request ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": 201450000001,
    "error_description": "Duplicate entry for key [personTypeId]"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `201250000001` | Correspondence for the specified payer type identifier already exists ||
|| `201240400002` | Payer type with the specified identifier does not exist ||
|| `200040300020` | Access error to the record ||
|| `100` | Parameter `fields` is not specified or is empty ||
|| `0` | Required fields are not provided ||
|| `0` | Other errors (e.g., fatal errors) ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./sale-business-value-person-domain-list.md)
- [{#T}](./sale-business-value-person-domain-delete-by-filter.md)
- [{#T}](./sale-business-value-person-domain-get-fields.md)