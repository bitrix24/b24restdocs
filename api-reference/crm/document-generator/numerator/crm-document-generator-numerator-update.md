# Update the Numerator crm.documentgenerator.numerator.update

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: user with "edit" access permission for document generator templates

The method `crm.documentgenerator.numerator.update` updates an existing numerator.

## Method Parameters

{% include [Parameter Note](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../data-types.md) | Identifier of the numerator ||
|| **fields***
[`object`](../../data-types.md) | Object with fields to update in the format:

```json
{
    "field_1": "value_1",
    "field_2": "value_2",
    "...": "..."
}
```

where:
- `field_n` — field name
- `value_n` — field value

The list of fields is [below](#parameter-fields) ||
|#

### Parameter fields {#parameter-fields}

{% include [Parameter Note](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **name**
[`string`](../../data-types.md) | Name of the numerator ||
|| **template**
[`string`](../../data-types.md) | Number template, for example `{NUMBER}` ||
|| **settings**
[`object`](../../data-types.md) | Generator settings. Parameter descriptions are [below](#parameter-settings) ||
|#

### Parameter settings {#parameter-settings}

#|
|| **Name**
`type` | **Description** ||
|| **Bitrix_Main_Numerator_Generator_SequentNumberGenerator**
[`object`](../../data-types.md) | Settings for sequential numbering. Parameter descriptions are [below](#parameter-sequent-settings) ||
|#

#### Parameters Bitrix_Main_Numerator_Generator_SequentNumberGenerator {#parameter-sequent-settings}

#|
|| **Name**
`type` | **Description** ||
|| **start**
[`integer`](../../data-types.md) | Initial value of the counter. Default is `1` ||
|| **step**
[`integer`](../../data-types.md) | Step for increasing the counter. Default is `1` ||
|| **length**
[`integer`](../../data-types.md) | Minimum length of the number. Default is `0` ||
|| **padString**
[`string`](../../data-types.md) | Padding character on the left when `length > 0`. Default is `'0'` ||
|| **periodicBy**
[`string`](../../data-types.md) | Counter reset period:
- `''` — no reset
- `day` — daily
- `month` — monthly
- `year` — yearly ||
|| **timezone**
[`string`](../../data-types.md) | Timezone identifier for periodic reset, for example `Europe/Berlin` ||
|| **isDirectNumeration**
[`boolean`](../../data-types.md) | Direct numbering flag. Default is `false` ||
|#

## Code Examples

{% include [Example Note](../../../../_includes/examples.md) %}

Example of updating a numerator:
- new template — `INV-{NUMBER}`
- start of the sequence — `100`
- step — `1`

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":45,"fields":{"name":"Numerator from REST (updated)","template":"INV-{NUMBER}","settings":{"Bitrix_Main_Numerator_Generator_SequentNumberGenerator":{"start":100,"step":1,"length":6,"padString":"0","periodicBy":"","timezone":"","isDirectNumeration":false}}}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.documentgenerator.numerator.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":45,"fields":{"name":"Numerator from REST (updated)","template":"INV-{NUMBER}","settings":{"Bitrix_Main_Numerator_Generator_SequentNumberGenerator":{"start":100,"step":1,"length":6,"padString":"0","periodicBy":"","timezone":"","isDirectNumeration":false}}},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.documentgenerator.numerator.update
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'crm.documentgenerator.numerator.update',
    		{
    			id: 45,
    			fields: {
    				name: 'Numerator from REST (updated)',
    				template: 'INV-{NUMBER}',
    				settings: {
    					Bitrix_Main_Numerator_Generator_SequentNumberGenerator: {
    						start: 100,
    						step: 1,
    						length: 6,
    						padString: '0',
    						periodicBy: '',
    						timezone: '',
    						isDirectNumeration: false,
    					},
    				},
    			},
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
                'crm.documentgenerator.numerator.update',
                [
                    'id' => 45,
                    'fields' => [
                        'name' => 'Numerator from REST (updated)',
                        'template' => 'INV-{NUMBER}',
                        'settings' => [
                            'Bitrix_Main_Numerator_Generator_SequentNumberGenerator' => [
                                'start' => 100,
                                'step' => 1,
                                'length' => 6,
                                'padString' => '0',
                                'periodicBy' => '',
                                'timezone' => '',
                                'isDirectNumeration' => false,
                            ],
                        ],
                    ],
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
        echo 'Error updating numerator: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'crm.documentgenerator.numerator.update',
        {
            id: 45,
            fields: {
                name: 'Numerator from REST (updated)',
                template: 'INV-{NUMBER}',
                settings: {
                    Bitrix_Main_Numerator_Generator_SequentNumberGenerator: {
                        start: 100,
                        step: 1,
                        length: 6,
                        padString: '0',
                        periodicBy: '',
                        timezone: '',
                        isDirectNumeration: false,
                    },
                },
            },
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
        'crm.documentgenerator.numerator.update',
        [
            'id' => 45,
            'fields' => [
                'name' => 'Numerator from REST (updated)',
                'template' => 'INV-{NUMBER}',
                'settings' => [
                    'Bitrix_Main_Numerator_Generator_SequentNumberGenerator' => [
                        'start' => 100,
                        'step' => 1,
                        'length' => 6,
                        'padString' => '0',
                        'periodicBy' => '',
                        'timezone' => '',
                        'isDirectNumeration' => false,
                    ],
                ],
            ],
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
        "name": "Numerator from REST (updated)",
        "template": "INV-{NUMBER}",
        "id": "45",
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
    },
    "time": {
        "start": 1773743992,
        "finish": 1773743992.664487,
        "duration": 0.6644868850708008,
        "processing": 0,
        "date_start": "2026-03-17T13:39:52+01:00",
        "date_finish": "2026-03-17T13:39:52+01:00",
        "operating_reset_at": 1773744592,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Root element of the response. Contains numerator data in the format [`result`](#result) ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

#### Type result {#result}

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
[`object`](../../data-types.md) | Saved generator settings ||
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
|| `100` | `Could not find value for parameter {fields}` | Required parameter `fields` not provided ||
|| `100` | `Bitrix\Main\Numerator\Numerator constructor must be is public` | Internal error while creating the numerator object ||
|| `100` | Invalid value {...} to match with parameter {fields}. Should be value of type array. | Parameter `fields` not passed as an array/object ||
|| `100` | `Could not construct parameter {numerator}` | Numerator with the specified `id` not found ||
|| `DOCGEN_ACCESS_ERROR` | `Access denied` | No access to the numerator. The method only updates numerators created via REST ||
|| `Empty value` | `You do not have permissions to modify templates` | Insufficient rights to modify document generator templates ||
|| `Empty value` | `Module documentgenerator is not installed` | The `documentgenerator` module is unavailable ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-document-generator-numerator-add.md)
- [{#T}](./crm-document-generator-numerator-get.md)
- [{#T}](./crm-document-generator-numerator-list.md)
- [{#T}](./crm-document-generator-numerator-delete.md)
- [{#T}](../../../../tutorials/crm/how-to-add-crm-objects/how-to-generate-documents.md)