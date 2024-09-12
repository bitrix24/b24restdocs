# Update Digital Workplace crm.automatedsolution.update

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- there is a link to the data-types file, check that the type crm_dynamic_type is described there (from SPAs)

{% endnote %}

{% endif %}

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can execute the method: users with administrative access to the crm section

This method updates the existing settings of the digital workplace with the identifier `id`. If any of the fields are not provided, their values will remain unchanged.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../data-types.md) | Identifier of the digital workplace. It can be obtained from the response of the method [crm.automatedsolution.add](./crm-automated-solution-add.md) (result.automatedSolution.id), which was called when adding the digital workplace, or [crm.automatedsolution.list](./crm-automated-solution-list.md). You can also use the "Digital Workplaces" section in the Bitrix24 account — the `ID` column in the list of digital workplaces ||
|| **fields***
[`object`](../data-types.md) | Field values (detailed description provided below) for creating a digital workplace in the following structure:

```js
"fields": {
    "title": "value",
    "typeIds": []
}
```
||
|#

### Parameter fields

#|
|| **Name**
`type` | **Description** ||
|| **title**
[`string`](../../data-types.md) | Title of the digital workplace.

Be careful when changing the `title` field. Since the link to the digital workplace is built based on the title, changing it will also change the link to the digital workplace ||
|| **typeIds**
[`crm_dynamic_type.id[]`](../data-types.md) | Array of identifiers of SPAs that need to be linked to this workplace.

To change the list of linked SPAs, you need to pass the `typeIds` field with the desired set of SPAs.

{% note warning %}

Settings are completely overwritten. When changing the list of linked SPAs, you must pass the `typeIds` set in its entirety, or omit the `typeIds` key altogether 

{% endnote %}

 ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

1. Change the title of the digital workplace

    {% list tabs %}

    - cURL (Webhook)

        ```bash
        curl -X POST \
        -H "Content-Type: application/json" \
        -H "Accept: application/json" \
        -d '{"id":238,"fields":{"title":"HR & Customer Success"}}' \
        https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.automatedsolution.update
        ```

    - cURL (OAuth)

        ```bash
        curl -X POST \
        -H "Content-Type: application/json" \
        -H "Accept: application/json" \
        -d '{"id":238,"fields":{"title":"HR & Customer Success"},"auth":"**put_access_token_here**"}' \
        https://**put_your_bitrix24_address**/rest/crm.automatedsolution.update
        ```

    - JS

        ```js
        BX24.callMethod(
            'crm.automatedsolution.update',
            {
                "id": 238,
                "fields": {
                    "title": "HR & Customer Success"
                }
            },
            function(result) {
                if (result.error()) {
                    console.error(result.error());
                } else {
                    console.info(result.data());
                }
            }
        );
        ```

    - PHP

        ```php
        require_once('crest.php');

        $result = CRest::call(
            'crm.automatedsolution.update',
            [
                'id' => 238,
                'fields' =>
                [
                    'title' => 'HR & Customer Success'
                ]
            ]
        );

        echo '<PRE>';
        print_r($result);
        echo '</PRE>';
        ```

    {% endlist %}

2. Change the list of linked SPAs

    Suppose the digital workplace with `id` = `267` had two SPAs linked — one with `id` = `14`, and the other with `id` = `158`. If we want the digital workplace to retain only one SPA, we pass the `typeIds` field containing only the desired SPAs:

    {% list tabs %}

    - cURL (Webhook)

        ```bash
        curl -X POST \
        -H "Content-Type: application/json" \
        -H "Accept: application/json" \
        -d '{"id":238,"fields":{"typeIds":[14]}}' \
        https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.automatedsolution.update
        ```

    - cURL (OAuth)

        ```bash
        curl -X POST \
        -H "Content-Type: application/json" \
        -H "Accept: application/json" \
        -d '{"id":238,"fields":{"typeIds":[14]},"auth":"**put_access_token_here**"}' \
        https://**put_your_bitrix24_address**/rest/crm.automatedsolution.update
        ```

    - JS

        ```js
        BX24.callMethod(
            'crm.automatedsolution.update',
            {
                "id": 238,
                "fields": {
                    "typeIds": [
                        14
                    ]
                }
            },
            function(result) {
                if (result.error()) {
                    console.error(result.error());
                } else {
                    console.info(result.data());
                }
            }
        );
        ```

    - PHP

        ```php
        require_once('crest.php');

        $result = CRest::call(
            'crm.automatedsolution.update',
            [
                'id' => 238,
                'fields' =>
                [
                    'typeIds' => [14]
                ]
            ]
        );

        echo '<PRE>';
        print_r($result);
        echo '</PRE>';
        ```

    {% endlist %}

3. Unlink all SPAs

    To unlink all SPAs from the digital workplace, you need to pass an empty array as `typeIds`.

    {% list tabs %}

    - cURL (Webhook)

        ```bash
        curl -X POST \
        -H "Content-Type: application/json" \
        -H "Accept: application/json" \
        -d '{"id":238,"fields":{"typeIds":[]}}' \
        https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.automatedsolution.update
        ```

    - cURL (OAuth)

        ```bash
        curl -X POST \
        -H "Content-Type: application/json" \
        -H "Accept: application/json" \
        -d '{"id":238,"fields":{"typeIds":[]},"auth":"**put_access_token_here**"}' \
        https://**put_your_bitrix24_address**/rest/crm.automatedsolution.update
        ```

    - JS

        ```js
        BX24.callMethod(
            'crm.automatedsolution.update',
            {
                "id": 238,
                "fields": {
                    "typeIds": []
                }
            },
            function(result) {
                if (result.error()) {
                    console.error(result.error());
                } else {
                    console.info(result.data());
                }
            }
        );
        ```

    - PHP

        ```php
        require_once('crest.php');

        $result = CRest::call(
            'crm.automatedsolution.update',
            [
                'id' => 238,
                'fields' =>
                [
                    'typeIds' => []
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
    "result": {
        "automatedSolution": {
            "id": 1,
            "title": "HR",
            "typeIds": [
                1,
                2,
                3
            ]
        }
    },
    "time": {
        "start": 1715849396.642359,
        "finish": 1715849396.954623,
        "duration": 0.31226396560668945,
        "processing": 0.0068209171295166016,
        "date_start": "2024-05-16T11:49:56+03:00",
        "date_finish": "2024-05-16T11:49:56+03:00",
        "operating_reset_at": 1715849996,
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
|| **automatedSolution**
[`object`](../../data-types.md) | Object containing information about the updated digital workplace ||
|| **time**
[`time`](../../data-types.md) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error":"BX_EMPTY_REQUIRED",
    "error_description":"Required field is not filled"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `ACCESS_DENIED` | Insufficient permissions ||
|| `BX_EMPTY_REQUIRED` | Required field is not filled ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./index.md)
- [{#T}](./crm-automated-solution-add.md)
- [{#T}](./crm-automated-solution-get.md)
- [{#T}](./crm-automated-solution-list.md)
- [{#T}](./crm-automated-solution-delete.md)
- [{#T}](./crm-automated-solution-fields.md)