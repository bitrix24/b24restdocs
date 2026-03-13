# Set a Set of Contacts Associated with the Specified Company crm.company.contact.items.set

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: user with "Edit" access permission for companies

The method `crm.company.contact.items.set` sets a set of contacts associated with the specified company.

## Method Parameters

{% include [Parameter Notes](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../../data-types.md) | Identifier of the company.

The identifier can be obtained using the methods [crm.company.list](../crm-company-list.md) or [crm.company.add](../crm-company-add.md)
||
|| **items***
[`object[]`](../../../data-types.md) | A set of objects that describe the associated contacts for the company. The structure of an individual binding object is detailed [below](#company_contact_binding) ||
|#

### Structure of the Binding Object {#company_contact_binding}

{% include [Parameter Notes](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **CONTACT_ID***
[`crm_entity`](../../data-types.md) | Identifier of the contact that will be associated with the company.

The identifier can be obtained using the method [crm.item.list](../../universal/crm-item-list.md) with `entityTypeId = 3` ||
|| **IS_PRIMARY**
[`char`](../../../data-types.md#char) | Indicates whether the binding is primary. Possible values:
- `Y` — yes
- `N` — no

If there is no binding with `IS_PRIMARY = Y`, it will be set for the first binding in `items`.

If multiple bindings with `IS_PRIMARY = Y` are provided, the first binding with `IS_PRIMARY = Y` will be considered primary.
||
|| **SORT**
[`integer`](../../../data-types.md) | Sort index.

By default, `i + 10`, where `i` is the maximum sort index of existing and provided bindings for the current company or `0` if `SORT` is not provided for any bindings and if the company has no bindings.

If an existing binding is provided without the `SORT` parameter, the default value will not be set; the value will remain the same. ||
|#

## Code Examples

{% include [Example Notes](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":32,"items":[{"CONTACT_ID":8,"IS_PRIMARY":"Y","SORT":100},{"CONTACT_ID":9,"SORT":200},{"CONTACT_ID":10,"SORT":400}]}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.company.contact.items.set
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":32,"items":[{"CONTACT_ID":8,"IS_PRIMARY":"Y","SORT":100},{"CONTACT_ID":9,"SORT":200},{"CONTACT_ID":10,"SORT":400}],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.company.contact.items.set
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'crm.company.contact.items.set',
    		{
    			id: 32,
    			items: [
    				{
    					CONTACT_ID: 8,
    					IS_PRIMARY: "Y",
    					SORT: 100,
    				},
    				{
    					CONTACT_ID: 9,
    					SORT: 200,
    				},
    				{
    					CONTACT_ID: 10,
    					SORT: 400,
    				}
    			],
    		}
    	);
    	
    	const result = response.getData().result;
    	result.error()
    		? console.error(result.error())
    		: console.info(result)
    	;
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
                'crm.company.contact.items.set',
                [
                    'id'    => 32,
                    'items' => [
                        [
                            'CONTACT_ID' => 8,
                            'IS_PRIMARY' => 'Y',
                            'SORT'       => 100,
                        ],
                        [
                            'CONTACT_ID' => 9,
                            'SORT'       => 200,
                        ],
                        [
                            'CONTACT_ID' => 10,
                            'SORT'       => 400,
                        ],
                    ],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        if ($result->error()) {
            echo 'Error: ' . $result->error();
        } else {
            echo 'Success: ' . print_r($result->data(), true);
            // Your logic for processing data
            processData($result->data());
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error setting contact items for company: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'crm.company.contact.items.set',
        {
            id: 32,
            items: [
                {
                    CONTACT_ID: 8,
                    IS_PRIMARY: "Y",
                    SORT: 100,
                },
                {
                    CONTACT_ID: 9,
                    SORT: 200,
                },
                {
                    CONTACT_ID: 10,
                    SORT: 400,
                }
            ],
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
        'crm.company.contact.items.set',
        [
            'id' => 32,
            'items' => [
                [
                    'CONTACT_ID' => 8,
                    'IS_PRIMARY' => 'Y',
                    'SORT' => 100,
                ],
                [
                    'CONTACT_ID' => 9,
                    'SORT' => 200,
                ],
                [
                    'CONTACT_ID' => 10,
                    'SORT' => 400,
                ]
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
        "start": 1724139480.073569,
        "finish": 1724139481.016709,
        "duration": 0.9431400299072266,
        "processing": 0.4230809211730957,
        "date_start": "2024-08-20T09:38:00+02:00",
        "date_finish": "2024-08-20T09:38:01+02:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../../data-types.md) | Root element of the response. Contains `true` in case of success ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the execution time of the request ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "",
    "error_description": "The parameter items must be array."
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `-`     | `The parameter ownerEntityID is invalid or not defined` | The provided `id` is less than or equal to 0 or not provided at all ||
|| `-`     | `The parameter items must be array` | The `items` parameter is not an array ||
|| `ACCESS_DENIED` | `Access denied!` | The user does not have permission to edit companies ||
|| `-`     | `Not found` | The company with the provided `id` was not found ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-company-contact-add.md)
- [{#T}](./crm-company-contact-delete.md)
- [{#T}](./crm-company-contact-fields.md)
- [{#T}](./crm-company-contact-items-get.md)
- [{#T}](./crm-company-contact-items-delete.md)