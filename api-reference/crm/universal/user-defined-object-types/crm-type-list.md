# Get a List of Custom Types crm.type.list

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user with administrative access to the CRM section

This method retrieves a list of smart process settings.

## Method Parameters

#|
|| **Name**
`type` | **Description** ||
|| **order**
[`object`][1]  | Object format: `{ field_1: value_1, field_2: value_2, ..., field_n: value_n }`, where
* `field_n`: the name of the field by which the smart processes will be sorted
* `value_n`: a `string` value that can be: 
  * `ASC` — ascending order
  * `DESC` — descending order

Possible values for `field` correspond to the fields of the [type](../../data-types.md#type) object
 ||
|| **filter**
[`object`][1]  | Object format: `{ field_1: value_1, field_2: value_2, ..., field_n: value_n }`, where
* `field_n`: the name of the field by which the smart processes will be filtered
* `value_n`: the filter value

Possible values for `field` correspond to the fields of the [type](../../data-types.md#type) object

The filter can have unlimited nesting and number of conditions.
By default, all conditions are combined using `AND`. If you need to use `OR`, you can pass a special key `logic` with the value `OR`.

You can add a prefix to the `field_n` keys to specify the filter operation.
Possible prefix values:
- `>=` — greater than or equal to
- `>` — greater than
- `<=` — less than or equal to
- `<` — less than
- `@` — IN, an array is passed as the value
- `!@` — NOT IN, an array is passed as the value
- `%` — LIKE, substring search. The `%` symbol should not be included in the filter value. The search looks for the substring in any position of the string
- `=%` — LIKE, substring search. The `%` symbol should be included in the value. Examples:
    - `"mol%"` — searches for values starting with "mol"
    - `"%mol"` — searches for values ending with "mol"
    - `"%mol%"` — searches for values where "mol" can be in any position
- `%=` — LIKE (similar to `=%`)
- `!%` — NOT LIKE, substring search. The `%` symbol should not be included in the filter value. The search goes from both sides
- `!=%` — NOT LIKE, substring search. The `%` symbol should be included in the value. Examples:
    - `"mol%"` — searches for values not starting with "mol"
    - `"%mol"` — searches for values not ending with "mol"
    - `"%mol%"` — searches for values where the substring "mol" is not present in any position
- `!%=` — NOT LIKE (similar to `!=%`)
- `=` — equal, exact match (used by default)
- `!=` — not equal
- `!` — not equal

 ||
|| **start**
[`integer`][1] | This parameter is used for pagination control.

The page size of results is always static — 50 records.

To select the second page of results, pass the value `50`. To select the third page of results — the value `100`, and so on.

The formula for calculating the `start` parameter value:

`start = (N-1) * 50`, where `N` — the desired page number ||
|#

{% include [Notes on parameters](../../../../_includes/required.md) %}

## Examples

{% include [Notes on examples](../../../../_includes/examples.md) %}

1. Get a list of all smart processes where `title` contains either `5` or `0`. Sort the resulting list in descending order by `id`.

    {% list tabs %}

    - cURL (Webhook)

        ```bash
        curl -X POST \
        -H "Content-Type: application/json" \
        -H "Accept: application/json" \
        -d '{"filter":[{"logic":"OR",{"%title":"5"},{"%title":"0"}]},"order":{"id":"DESC"}}' \
        https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.type.list
        ```

    - cURL (OAuth)

        ```bash
        curl -X POST \
        -H "Content-Type: application/json" \
        -H "Accept: application/json" \
        -d '{"filter":[{"logic":"OR",{"%title":"5"},{"%title":"0"}]},"order":{"id":"DESC"},"auth":"**put_access_token_here**"}' \
        https://**put_your_bitrix24_address**/rest/crm.type.list
        ```

    - JS

        ```js
        BX24.callMethod(
            'crm.type.list', 
            {
                filter: {
                    "0": {
                        logic: 'OR',
                        "0": {
                            "%title": "5",
                        },
                        "1": {
                            "%title": "0",
                        },
                    },
                },
                order: {
                    id: 'DESC',
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
            'crm.type.list',
            [
                'filter' => [
                    [
                        'logic' => 'OR',
                        [
                            '%title' => '5',
                        ],
                        [
                            '%title' => '0',
                        ],
                    ],
                ],
                'order' => [
                    'id' => 'DESC',
                ],
            ]
        );

        echo '<PRE>';
        print_r($result);
        echo '</PRE>';
        ```

    {% endlist %}

2. Get a list of smart processes where:
   - Automation rules and triggers are enabled (`isAutomationEnabled`)
   - Business process designer is enabled (`isBizProcEnabled`)
   - Custom sales funnels and tunnels are enabled (`isCategoriesEnabled`)
   - Custom stages and kanban are enabled (`isClientEnabled`)

    {% list tabs %}

    - cURL (Webhook)

        ```bash
        curl -X POST \
        -H "Content-Type: application/json" \
        -H "Accept: application/json" \
        -d '{"filter":{"isAutomationEnabled":"Y","isBizProcEnabled":"Y","isCategoriesEnabled":"Y","isClientEnabled":"Y"}}' \
        https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.type.list
        ```

    - cURL (OAuth)

        ```bash
        curl -X POST \
        -H "Content-Type: application/json" \
        -H "Accept: application/json" \
        -d '{"filter":{"isAutomationEnabled":"Y","isBizProcEnabled":"Y","isCategoriesEnabled":"Y","isClientEnabled":"Y"},"auth":"**put_access_token_here**"}' \
        https://**put_your_bitrix24_address**/rest/crm.type.list
        ```

    - JS

        ```js
        BX24.callMethod(
            'crm.type.list',
            {
                filter: {
                    isAutomationEnabled: 'Y',
                    isBizProcEnabled: "Y",
                    isCategoriesEnabled: "Y",
                    isClientEnabled: "Y",
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
            'crm.type.list',
            [
                'filter' => [
                    'isAutomationEnabled' => 'Y',
                    'isBizProcEnabled' => 'Y',
                    'isCategoriesEnabled' => 'Y',
                    'isClientEnabled' => 'Y',
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
    "result": {
        "types": [
            {
                "id": 37,
                "title": "Smart Process #20",
                "code": "",
                "createdBy": 1,
                "entityTypeId": 1256,
                "customSectionId": null,
                "isCategoriesEnabled": "Y",
                "isStagesEnabled": "N",
                "isBeginCloseDatesEnabled": "Y",
                "isClientEnabled": "Y",
                "isUseInUserfieldEnabled": "Y",
                "isLinkWithProductsEnabled": "N",
                "isMycompanyEnabled": "Y",
                "isDocumentsEnabled": "Y",
                "isSourceEnabled": "Y",
                "isObserversEnabled": "Y",
                "isRecyclebinEnabled": "Y",
                "isAutomationEnabled": "Y",
                "isBizProcEnabled": "N",
                "isSetOpenPermissions": "N",
                "isPaymentsEnabled": "N",
                "isCountersEnabled": "N",
                "createdTime": "2024-07-08T17:24:55+02:00",
                "updatedTime": "2024-07-08T17:24:55+02:00",
                "updatedBy": 1
            },
            {
                "id": 32,
                "title": "Smart Process #15",
                "code": "",
                "createdBy": 1,
                "entityTypeId": 1246,
                "customSectionId": null,
                "isCategoriesEnabled": "N",
                "isStagesEnabled": "Y",
                "isBeginCloseDatesEnabled": "N",
                "isClientEnabled": "Y",
                "isUseInUserfieldEnabled": "Y",
                "isLinkWithProductsEnabled": "Y",
                "isMycompanyEnabled": "N",
                "isDocumentsEnabled": "N",
                "isSourceEnabled": "N",
                "isObserversEnabled": "N",
                "isRecyclebinEnabled": "N",
                "isAutomationEnabled": "N",
                "isBizProcEnabled": "Y",
                "isSetOpenPermissions": "Y",
                "isPaymentsEnabled": "N",
                "isCountersEnabled": "N",
                "createdTime": "2024-07-08T17:24:52+02:00",
                "updatedTime": "2024-07-08T17:24:52+02:00",
                "updatedBy": 1
            },
            {
                "id": 27,
                "title": "Smart Process #10",
                "code": "",
                "createdBy": 1,
                "entityTypeId": 1236,
                "customSectionId": null,
                "isCategoriesEnabled": "N",
                "isStagesEnabled": "Y",
                "isBeginCloseDatesEnabled": "Y",
                "isClientEnabled": "Y",
                "isUseInUserfieldEnabled": "N",
                "isLinkWithProductsEnabled": "N",
                "isMycompanyEnabled": "Y",
                "isDocumentsEnabled": "N",
                "isSourceEnabled": "Y",
                "isObserversEnabled": "N",
                "isRecyclebinEnabled": "N",
                "isAutomationEnabled": "Y",
                "isBizProcEnabled": "Y",
                "isSetOpenPermissions": "Y",
                "isPaymentsEnabled": "N",
                "isCountersEnabled": "N",
                "createdTime": "2024-07-08T17:24:50+02:00",
                "updatedTime": "2024-07-08T17:24:50+02:00",
                "updatedBy": 1
            },
            {
                "id": 22,
                "title": "Smart Process #5",
                "code": "",
                "createdBy": 1,
                "entityTypeId": 1226,
                "customSectionId": null,
                "isCategoriesEnabled": "Y",
                "isStagesEnabled": "N",
                "isBeginCloseDatesEnabled": "N",
                "isClientEnabled": "N",
                "isUseInUserfieldEnabled": "Y",
                "isLinkWithProductsEnabled": "Y",
                "isMycompanyEnabled": "N",
                "isDocumentsEnabled": "N",
                "isSourceEnabled": "Y",
                "isObserversEnabled": "Y",
                "isRecyclebinEnabled": "Y",
                "isAutomationEnabled": "N",
                "isBizProcEnabled": "Y",
                "isSetOpenPermissions": "Y",
                "isPaymentsEnabled": "N",
                "isCountersEnabled": "N",
                "createdTime": "2024-07-08T17:24:48+02:00",
                "updatedTime": "2024-07-08T17:24:48+02:00",
                "updatedBy": 1
            }
        ]
    },
    "time": {
        "start": 1720516793.835427,
        "finish": 1720516794.697913,
        "duration": 0.8624858856201172,
        "processing": 0.07323503494262695,
        "date_start": "2024-07-09T11:19:53+02:00",
        "date_finish": "2024-07-09T11:19:54+02:00",
        "operating": 0
    },
    "total": 4
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`][1] | The root element of the response. Contains a single key `types` ||
|| **types**
[`type[]`](../../data-types.md#type) | A list of smart processes, each corresponding to the structure of the [type](../../data-types.md#type) object ||
|| **time**
[`time`][1] | Information about the request execution time ||
|| **total**
[`integer`][1]| The total number of records found || 
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "ACCESS_DENIED",
    "error_description": "Access denied"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes
#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `403` | `allowed_only_intranet_user` | Action allowed only for intranet users | Occurs if the user is not an intranet user ||
|| `400` | `ACCESS_DENIED` | Access denied | Occurs if the user does not have administrative rights in CRM ||
|| `400` | `INVALID_ARG_VALUE` | Invalid filter: field `'field_n'` is not allowed in filter | Occurs when a non-existent field `field_n` is passed in the `filter` parameter ||
|| `400` | `INVALID_ARG_VALUE` | Invalid filter: field `'field_n'` has invalid value | Occurs when an incorrect `value_n` is passed for the field `field_n` in the `filter` parameter ||
|| `400` | `INVALID_ARG_VALUE` | Invalid order: field `'field_n'` is not allowed in order | Occurs when a non-existent field `field_n` is passed in the `order` parameter ||
|| `400` | `INVALID_ARG_VALUE` | Invalid order: allowed sort directions are `ASC, DESC`. But got `'invalid_value'` for field `'field_n'` | Occurs when an incorrect `value_n` is passed for the field `field_n` in the `order` parameter ||
|#

{% include [system errors](./../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./crm-type-add.md)
- [{#T}](./crm-type-update.md)
- [{#T}](./crm-type-get.md)
- [{#T}](./crm-type-get-by-entity-type-id.md)
- [{#T}](./crm-type-delete.md)
- [{#T}](./crm-type-fields.md)
- [{#T}](../../../../tutorials/crm/how-to-add-crm-objects/how-to-add-user-field-to-spa.md)

[1]: ../../../data-types.md