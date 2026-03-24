# Get Information about the Numerator crm.documentgenerator.numerator.get

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: user with "edit" access permission for document generator templates

The method `crm.documentgenerator.numerator.get` returns information about the numerator by its identifier.

## Method Parameters

{% include [Footnote on parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../data-types.md) | Identifier of the numerator ||
|#

## Code Examples

{% include [Footnote on examples](../../../../_includes/examples.md) %}

Example of retrieving the numerator with `id = 45`.

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":45}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.documentgenerator.numerator.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":45,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.documentgenerator.numerator.get
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'crm.documentgenerator.numerator.get',
    		{
    			id: 45,
    		}
    	);

    	const result = response.getData().result;
    	console.info(result);
    }
    catch (error)
    {
    	console.error('Error:', error);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.documentgenerator.numerator.get',
                [
                    'id' => 45,
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo '<pre>';
        print_r($result);
        echo '</pre>';

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting numerator: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'crm.documentgenerator.numerator.get',
        {
            id: 45,
        },
        (result) => {
            result.error()
                ? console.error(result.error())
                : console.info(result.data())
            ;
        },
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.documentgenerator.numerator.get',
        [
            'id' => 45,
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
        "numerator": {
            "id": "45",
            "name": "Numerator from REST (updated)",
            "template": "INV-{NUMBER}",
            "code": null,
            "settings": {
                "Bitrix_Main_Numerator_Generator_SequentNumberGenerator": {
                    "start": 100,
                    "step": 1,
                    "length": 0,
                    "padString": "0",
                    "periodicBy": null,
                    "timezone": null,
                    "isDirectNumeration": false
                }
            }
        }
    },
    "time": {
        "start": 1773747475,
        "finish": 1773747475.904903,
        "duration": 0.9049029350280762,
        "processing": 0,
        "date_start": "2026-03-17T14:37:55+02:00",
        "date_finish": "2026-03-17T14:37:55+02:00",
        "operating_reset_at": 1773748075,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Root element of the response. Contains the [`numerator`](#numerator) object ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

#### Type numerator {#numerator}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`string`](../../data-types.md) | Identifier of the numerator ||
|| **name**
[`string`](../../data-types.md) | Name of the numerator ||
|| **template**
[`string`](../../data-types.md) | Number template ||
|| **code**
[`string`](../../data-types.md) | Symbolic code of the numerator. Can be `null` ||
|| **settings**
[`object`](../../data-types.md) | Saved settings of the generators ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "100",
    "error_description": "Could not construct parameter {numerator}"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `100` | `Bitrix\Main\Numerator\Numerator constructor must be is public` | Error creating the numerator object ||
|| `100` | `Could not construct parameter {numerator}` | Numerator with the specified `id` not found ||
|| `DOCGEN_ACCESS_ERROR` | `Access denied` | No access to the numerator, or the numerator does not belong to the document generator module ||
|| `Empty value` | `You do not have permissions to modify templates` | Insufficient permissions to modify document generator templates ||
|| `Empty value` | `Module documentgenerator is not installed` | The `documentgenerator` module is unavailable ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-document-generator-numerator-add.md)
- [{#T}](./crm-document-generator-numerator-update.md)
- [{#T}](./crm-document-generator-numerator-list.md)
- [{#T}](./crm-document-generator-numerator-delete.md)
- [{#T}](../../../../tutorials/crm/how-to-add-crm-objects/how-to-generate-documents.md)