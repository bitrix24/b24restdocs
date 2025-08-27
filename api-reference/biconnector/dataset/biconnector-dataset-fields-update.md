# Update Dataset Fields biconnector.dataset.fields.update

> Scope: [`biconnector`](../../scopes/permissions.md)
>
> Who can execute the method: user with access to the "Analyst's workspace" section

The method `biconnector.dataset.fields.update` updates the fields of an existing dataset.

## Method Parameters

{% include [Note on parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../data-types.md) | Identifier of the dataset, can be obtained using the methods [biconnector.dataset.list](./biconnector-dataset-list.md) or [biconnector.dataset.add](./biconnector-dataset-add.md) ||
|| **add**
[`object`](../../data-types.md) | Object containing an array of fields to be added with the following structure: 

```
{
    "type" : "int",
    "name": "NAME",
    "externalCode":"NAME"
},
```

- `type` — field type
- `name` — field name
- `externalCode` — external code of the field ||
|| **update**
[`object`](../../data-types.md) | Object containing an array of fields to be updated with the following structure:

```
{
    "id": 12,
    "visible": false
},
```

- `id` — Identifier of the field, can be obtained using the method [biconnector.dataset.get](./biconnector-dataset-get.md)
- `visible` — visibility of the field ||
|| **delete**
[`int[]`](../../data-types.md) | Object containing an array of field identifiers for deletion. Field identifiers can be obtained using the method [biconnector.dataset.get](./biconnector-dataset-get.md) ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{
        "id": 10,
        "add": [
            {
                "type": "int",
                "name": "NAME",
                "externalCode": "NAME"
            },
            {
                "type": "int",
                "name": "ID",
                "externalCode": "ID"
            }
        ],
        "update": [
            {
                "id": 12,
                "visible": false
            },
            {
                "id": 13,
                "visible": true
            }
        ],
        "delete": [
            14,
            15
        ]
    }' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/biconnector.dataset.fields.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{
        "id": 10,
        "add": [
            {
                "type": "int",
                "name": "NAME",
                "externalCode": "NAME"
            },
            {
                "type": "int",
                "name": "ID",
                "externalCode": "ID"
            }
        ],
        "update": [
            {
                "id": 12,
                "visible": false
            },
            {
                "id": 13,
                "visible": true
            }
        ],
        "delete": [
            14,
            15
        ],
        "auth": "**put_access_token_here**"
    }' \
    https://**put_your_bitrix24_address**/rest/biconnector.dataset.fields.update
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'biconnector.dataset.fields.update',
            {
                id: 10,
                add: [
                    {
                        type: "int",
                        name: "NAME",
                        externalCode: "NAME"
                    },
                    {
                        type: "int",
                        name: "ID",
                        externalCode: "ID"
                    }
                ],
                update: [
                    {
                        "id": 12,
                        "visible": false
                    },
                    {
                        "id": 13,
                        "visible": true
                    }
                ],
                delete: [
                    14,
                    15
                ]
            }
        );
        
        const result = response.getData().result;
        result.error()
            ? console.error(result.error())
            : console.info(result);
    }
    catch( error )
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
                'biconnector.dataset.fields.update',
                [
                    'id'     => 10,
                    'add'    => [
                        [
                            'type'         => "int",
                            'name'         => "NAME",
                            'externalCode' => "NAME"
                        ],
                        [
                            'type'         => "int",
                            'name'         => "ID",
                            'externalCode' => "ID"
                        ]
                    ],
                    'update' => [
                        [
                            'id'      => 12,
                            'visible' => false
                        ],
                        [
                            'id'      => 13,
                            'visible' => true
                        ]
                    ],
                    'delete' => [14, 15]
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        if ($result->error()) {
            echo 'Error: ' . $result->error();
        } else {
            echo 'Data: ' . print_r($result->data(), true);
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error updating dataset fields: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'biconnector.dataset.fields.update',
        {
            id: 10,
            add: [
                {
                    type: "int",
                    name: "NAME",
                    externalCode: "NAME"
                },
                {
                    type: "int",
                    name: "ID",
                    externalCode: "ID"
                }
            ],
            update: [
                {
                    "id": 12,
                    "visible": false
                },
                {
                    "id": 13,
                    "visible": true
                }
            ],
            delete: [
                14,
                15
            ]
        },
        (result) => {
            result.error()
                ? console.error(result.error())
                : console.info(result.data());
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'biconnector.dataset.fields.update',
        [
            'id' => 10,
            'add' => [
                [
                    'type' => 'int',
                    'name' => 'NAME',
                    'externalCode' => 'NAME'
                ],
                [
                    'type' => 'int',
                    'name' => 'ID',
                    'externalCode' => 'ID'
                ]
            ],
            'update' => [
                [
                    'id' => 12,
                    'visible' => false
                ],
                [
                    'id' => 13,
                    'visible' => true
                ]
            ],
            'delete' => [14, 15]
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
        "start": 1725365418.056843,
        "finish": 1725365419.671506,
        "duration": 1.6146628856658936,
        "processing": 1.3475170135498047,
        "date_start": "2024-09-03T14:10:18+02:00",
        "date_finish": "2024-09-03T14:10:19+02:00"
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../data-types.md) | Root element of the response, contains `true` in case of success ||
|| **time**
[`time`](../../data-types.md#time) | Information about the execution time of the request ||
|#

## Error Handling

HTTP status: **200**

```json
{
    "error": "VALIDATION_ID_NOT_PROVIDED",
    "error_description": "ID is missing."
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `VALIDATION_ID_NOT_PROVIDED` | ID is missing. | Identifier is not specified ||
|| `VALIDATION_INVALID_ID_FORMAT` | ID has to be a positive integer. | Invalid ID format ||
|| `DATASET_NOT_FOUND` | Dataset was not found. | Dataset not found ||
|| `DATASET_UPDATE_ERROR` | Error updating dataset. | Error updating dataset ||
|| `VALIDATION_DUPLICATE_FIELD_CODE` | Duplicate values found in the "code" parameter: #LIST_CODES# | Duplicates found in the `externalCode` parameter of dataset fields ||
|| `VALIDATION_DUPLICATE_FIELD_NAME` | Duplicate values found in the "name" parameter: #LIST_NAMES# | Duplicates found in the `name` parameter of dataset fields ||
|| `VALIDATION_FIELD_NAME_INVALID_FORMAT` | Field "name" has to start with an uppercase Latin character. Possible entry includes uppercase Latin characters (A-Z), numbers (0-9) and underscores. | Incorrect field name format. The name must start with a letter, only uppercase Latin letters `(A-Z)`, numbers, and `_` are allowed ||
|| `VALIDATION_FIELD_NAME_TOO_LONG` | Field "name" must not exceed 32 characters. | Field name must not exceed 32 characters ||
|| `VALIDATION_FIELD_INVALID_TYPE` | Invalid field type. | Incorrect field type ||
|| `VALIDATION_DUPLICATE_EXIST_CODE` | The following "externalCode" values already exist in the current fields: #LIST_CODES# | Fields with this `externalCode` parameter already exist ||
|| `VALIDATION_DUPLICATE_EXIST_NAME` | The following "name" values already exist in the current fields: #LIST_NAMES# | Fields with this `name` parameter already exist ||
|| `VALIDATION_FIELD_ADD_MISSING_REQUIRED_FIELDS` | Field to be added must include the required parameters: "name", "externalCode" and "type". | Field to be added must include `name`, `externalCode`, and `type` parameters ||
|| `VALIDATION_FIELD_UPDATE_MISSING_REQUIRED_FIELDS` | Field to be updated must include the required parameters: "id" and "visible". | Field to be updated must include `id` and `visible` parameters ||
|| `VALIDATION_FIELD_DELETE_INVALID_ID` | ID to be deleted must be a positive integer. | Invalid `id` format for deletion ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./biconnector-dataset-add.md)
- [{#T}](./biconnector-dataset-update.md)
- [{#T}](./biconnector-dataset-fields.md)
- [{#T}](./biconnector-dataset-get.md)
- [{#T}](./biconnector-dataset-list.md)
- [{#T}](./biconnector-dataset-delete.md)