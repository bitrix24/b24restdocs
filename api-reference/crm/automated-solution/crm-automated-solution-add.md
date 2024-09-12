# Create Digital Workspace crm.automatedsolution.add

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- there is a link to the data-types file, need to check that the type crm_dynamic_type is described (from SPAs)

{% endnote %}

{% endif %}

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can execute the method: users with administrative access to the CRM section

This method will create a new digital workspace.

In the cloud, the maximum number of digital workspaces depends on the plan. In on-premise, the maximum number depends on technical limitations.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields***
[`object`](../../data-types.md) | Field values (detailed description provided below) for creating a digital workspace in the form of a structure:

```js
"fields": {
    "title": "value",
    "typeIds": []
}
```
 ||
|#

### Parameter fields

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **title***
[`string`](../../data-types.md) | The name of the digital workspace. The link to the corresponding section on the account will be built based on the title of the digital workspace. ||
|| **typeIds**
[`crm_dynamic_type.id[]`](../data-types.md) | An array of identifiers of SPAs that need to be linked to this workspace.

If the SPA was previously linked to another workspace or to the CRM, it will disappear from there after being linked to the new workspace.

A digital workspace without SPAs will not be displayed in the left menu. However, it can be found in the list of digital workspaces. ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

1. Create a digital workspace and immediately link SPAs to it

    {% list tabs %}

    - cURL (Webhook)

        ```bash
        curl -X POST \
        -H "Content-Type: application/json" \
        -H "Accept: application/json" \
        -d '{"fields":{"title":"HR","typeIds":[1,2,3]}}' \
        https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.automatedsolution.add
        ```

    - cURL (OAuth)

        ```bash
        curl -X POST \
        -H "Content-Type: application/json" \
        -H "Accept: application/json" \
        -d '{"fields":{"title":"HR","typeIds":[1,2,3]},"auth":"**put_access_token_here**"}' \
        https://**put_your_bitrix24_address**/rest/crm.automatedsolution.add
        ```

    - JS

        ```js
        BX24.callMethod(
            'crm.automatedsolution.add',
            {
                "fields": {
                    "title": "HR",
                    "typeIds": [
                        1,
                        2,
                        3
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
            'crm.automatedsolution.add',
            [
                'fields' =>
                [
                    'title' => 'HR',
                    'typeIds' => [1, 2, 3]
                ]
            ]
        );

        echo '<PRE>';
        print_r($result);
        echo '</PRE>';
        ```

    {% endlist %}

2. Create a digital workspace without SPAs

    {% list tabs %}

    - cURL (Webhook)

        ```bash
        curl -X POST \
        -H "Content-Type: application/json" \
        -H "Accept: application/json" \
        -d '{"fields":{"title":"HR"}}' \
        https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.automatedsolution.add
        ```

    - cURL (OAuth)

        ```bash
        curl -X POST \
        -H "Content-Type: application/json" \
        -H "Accept: application/json" \
        -d '{"fields":{"title":"HR"},"auth":"**put_access_token_here**"}' \
        https://**put_your_bitrix24_address**/rest/crm.automatedsolution.add
        ```

    - JS

        ```js
        BX24.callMethod(
            'crm.automatedsolution.add',
            {
                "fields": {
                    "title": "HR"
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
            'crm.automatedsolution.add',
            [
                'fields' =>
                [
                    'title' => 'HR'
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
[`object`](../../data-types.md) | Object containing information about the added digital workspace ||
|| **time**
[`time`](../../data-types.md) | Information about the execution time of the request ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "BX_EMPTY_REQUIRED",
    "error_description":"The required field 'Title' is not filled"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `ACCESS_DENIED` | Insufficient permissions ||
|| `LIMIT_EXCEEDED` | The number of available digital workspaces has been exceeded ||
|| `BX_EMPTY_REQUIRED` | A required field is not filled ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./index.md)
- [{#T}](./crm-automated-solution-update.md)
- [{#T}](./crm-automated-solution-get.md)
- [{#T}](./crm-automated-solution-list.md)
- [{#T}](./crm-automated-solution-delete.md)
- [{#T}](./crm-automated-solution-fields.md)