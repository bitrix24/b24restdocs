# Set Common Detail for All Users crm.item.details.configuration.forceCommonScopeForAll

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: Administrator

This method forcibly sets a common detail for all users, removing their personal detail settings.

{% include [Extras Notice](./_includes/extras_notice.md) %}

## Method Parameters

{% include [Parameters Note](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **entityTypeId***
[`integer`][1] | Identifier of the [system](./../../index.md) or [user-defined type](./../user-defined-object-types/index.md) of CRM objects ||
|| **extras**
[`object`][1] | Additional parameters. Possible values and their structure are described [below](#extras) ||
|#

### extras

The `extras` parameter depends on the CRM object.

#|
|| **CRM Object** | **Name** | **Description** ||
|| **SPA** | `categoryId` | Identifier of the SPA funnel. Can be obtained using [`crm.category.list`](./../category/crm-category-list.md).

If not specified, the default funnel identifier for this SPA is used. ||
|| **Deal** | `dealCategoryId` | Identifier of the deal funnel. Can be obtained using [`crm.category.list`](./../category/crm-category-list.md).

If not specified, the default funnel identifier for deals is used. ||
|| **Lead** | `leadCustomerType` | Type of leads. 

Possible values:
- `1` — simple leads
- `2` — repeat leads
||
|#

## Code Examples

{% include [Examples Note](../../../../_includes/examples.md) %}

Set a common detail for deals in the funnel with `id = 9`

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"entityTypeId":2,"extras":{"dealCategoryId":9}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.item.details.configuration.forceCommonScopeForAll
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"entityTypeId":2,"extras":{"dealCategoryId":9},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.item.details.configuration.forceCommonScopeForAll
    ```

- JS

    ```js
    BX24.callMethod(
        'crm.item.details.configuration.forceCommonScopeForAll',
        {
            entityTypeId: 2,
            extras: {
                dealCategoryId: 9,
            },
        },
        (result) => {
            if (result.error())
            {
                console.error(result.error());

                return;
            }

            console.info(result.data());
        },
    );
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.item.details.configuration.forceCommonScopeForAll',
        [
            'entityTypeId' => 2,
            'extras' => [
                'dealCategoryId' => 9,
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
    "result": true,
    "time": {
        "start": 1720687903.685834,
        "finish": 1720687904.076471,
        "duration": 0.3906371593475342,
        "processing": 0.02508091926574707,
        "date_start": "2024-07-11T10:51:43+02:00",
        "date_finish": "2024-07-11T10:51:44+02:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`][1] | Root element of the response. Returns `true` on success ||
|| **time**
[`time`][1] | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "",
    "error_description": "Parameter 'entityTypeID' is not defined"
}
```

{% include notitle [Error Handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| Empty value | Parameter 'entityTypeID' is not defined | Required parameter `entityTypeId` is not provided ||
|| Empty value | The entity type '`entityTypeName`' is not supported in the current context. | The method does not support this entity type ||
|| Empty value | Access denied. | The user does not have administrative rights ||
|#

{% include [System Errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./crm-item-details-configuration-get.md)
- [{#T}](./crm-item-details-configuration-set.md)
- [{#T}](./crm-item-details-configuration-reset.md)

[1]: ../../../data-types.md