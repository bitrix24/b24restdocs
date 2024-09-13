# Reset Item Card Parameters crm.item.details.configuration.reset

> Method name: **crm.item.details.configuration.reset**
>
> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: access permission checks depend on the provided data:
>   - Any user has the right to reset their personal settings
>   - A user can reset shared and others' settings only if they are an administrator

This method resets the item card settings to their default values. It removes the personal settings of the specified user or the shared settings defined for all users.

{% include [Extras Notice](./_includes/extras_notice.md) %}

## Method Parameters

{% include [Required Parameters Note](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **entityTypeId***
[`integer`][1] | Identifier of the [system](./../../index.md) or [user-defined type](./../user-defined-object-types/index.md) of CRM objects ||
|| **userId**
[`user`][1] | Identifier of the user whose configuration you want to reset.

If the parameter is not provided, the `userId` of the user calling this method will be used.

Required only when requesting personal settings
||
|| **scope**
[`string`][1] | Scope of the settings. Allowed values:
- `'P'` — personal settings
- `'C'` — shared settings

The default value is `'P'` ||
|| **extras**
[`object`][1] | Additional parameters. Possible values and their structure are described [below](#extras) ||
|#

### extras

The `extras` parameter depends on the CRM object.

#|
|| **CRM Object** | **Name** | **Description** ||
|| **SPA** | `categoryId` | Identifier of the sales funnel for SPAs. Can be obtained using [`crm.category.list`](./../category/crm-category-list.md).

If not specified, the default funnel identifier for this SPA will be used ||
|| **Deal** | `dealCategoryId` | Identifier of the sales funnel for deals. Can be obtained using [`crm.category.list`](./../category/crm-category-list.md).

If not specified, the default funnel identifier for deals will be used ||
|| **Lead** | `leadCustomerType` | Type of leads. 

Possible values:
- `1` — simple leads
- `2` — repeat leads
||
|#

## Code Examples

{% include [Examples Note](../../../../_includes/examples.md) %}

Reset the shared configuration for deal cards in the funnel with `id = 9` for the user with `id = 1`

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"entityTypeId":2,"userId":1,"scope":"C","extras":{"dealCategoryId":9}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.item.details.configuration.reset
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"entityTypeId":2,"userId":1,"scope":"C","extras":{"dealCategoryId":9},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.item.details.configuration.reset
    ```

- JS

    ```js
        BX24.callMethod(
            'crm.item.details.configuration.reset',
            {
                entityTypeId: 2,
                userId: 1,
                scope: "C",
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
        'crm.item.details.configuration.reset',
        [
            'entityTypeId' => 2,
            'userId' => 1,
            'scope' => 'C',
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

HTTP status: **200**

```json
{
    "result": true,
    "time": {
        "start": 1720687072.190654,
        "finish": 1720687072.586945,
        "duration": 0.39629101753234863,
        "processing": 0.057084083557128906,
        "date_start": "2024-07-11T10:37:52+02:00",
        "date_finish": "2024-07-11T10:37:52+02:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`][1] | Root element of the response. Returns `true` if the settings were successfully reset ||
|| **time**
[`time`][1] | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

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
|| Empty value | Parameter 'entityTypeID' is not defined | Required parameter `entityTypeId` not provided ||
|| Empty value | The entity type '`entityTypeName`' is not supported in current context. | The method does not support this entity type ||
|| Empty value | Access denied. | The user does not have administrative rights ||
|#

{% include [System Errors](./../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./crm-item-details-configuration-get.md)
- [{#T}](./crm-item-details-configuration-set.md)
- [{#T}](./crm-item-details-configuration-forceCommonScopeForAll.md)

[1]: ../../../data-types.md