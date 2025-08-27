# Add a Company to the Specified Contact crm.contact.company.add

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user with "edit" access permission for contacts

The method `crm.contact.company.add` adds a company to the specified contact.

## Method Parameters

{% include [Note on parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`][1] | Identifier of the contact.

Can be obtained using the methods [crm.contact.list](../crm-contact-list.md) or [crm.contact.add](../crm-contact-add.md)
||
|| **fields***
[`object`][1] | Object format:

```
{
    field_1: value_1,
    field_2: value_2,
    ...,
    field_n: value_n,
}
```

where:
- `field_n` — field name
- `value_n` — field value

The list of available fields is described [below](#parameter-fields). ||
|#

### Parameter fields {#parameter-fields}

{% include [Note on parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **COMPANY_ID***
[`crm_entity`][2] | Identifier of the company that will be linked to the contact.

The identifier can be obtained using the method [crm.item.list](../../universal/crm-item-list.md) with `entityTypeId = 4` ||
|| **IS_PRIMARY**
[`boolean`][1] | Indicates whether the link is primary. Possible values:
- `Y` — yes
- `N` — no

For the first added element, `IS_PRIMARY` is `Y` by default.

Passing `IS_PRIMARY = Y` for a new and not first link overrides the existing primary link ||
|| **SORT**
[`integer`][1] | Sort index.

By default, `i + 10`, where `i` is the maximum sort index of existing links for the current contact or `0` if there are none ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

Example of adding a contact-company link, where:
- contact identifier — `54`
- company identifier — `32`

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":54,"fields":{"COMPANY_ID":32,"IS_PRIMARY":"Y","SORT":1000}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.contact.company.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":54,"fields":{"COMPANY_ID":32,"IS_PRIMARY":"Y","SORT":1000},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.contact.company.add
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'crm.contact.company.add',
    		{
    			id: 54,
    			fields: {
    				COMPANY_ID: 32,
    				IS_PRIMARY: "Y",
    				SORT: 1000,
    			},
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
                'crm.contact.company.add',
                [
                    'id' => 54,
                    'fields' => [
                        'COMPANY_ID' => 32,
                        'IS_PRIMARY' => 'Y',
                        'SORT' => 1000,
                    ],
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
        echo 'Error adding contact company: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'crm.contact.company.add',
        {
            id: 54,
            fields: {
                COMPANY_ID: 32,
                IS_PRIMARY: "Y",
                SORT: 1000,
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
        'crm.contact.company.add',
        [
            'id' => 54,
            'fields' => [
                'COMPANY_ID' => 32,
                'IS_PRIMARY' => 'Y',
                'SORT' => 1000,
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
        "start": 1724068028.331234,
        "finish": 1724068028.726591,
        "duration": 0.3953571319580078,
        "processing": 0.13033390045166016,
        "date_start": "2024-08-19T13:47:08+02:00",
        "date_finish": "2024-08-19T13:47:08+02:00"
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`][1] | Root element of the response. Contains:
- `true` — on success
- `false` — on failure (most likely the company you are trying to add is already linked)
||
|| **time**
[`time`][1] | Information about the execution time of the request ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "",
    "error_description": "The parameter 'ownerEntityID' is invalid or not defined."
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `-`     | `The parameter 'ownerEntityID' is invalid or not defined` | The `id` is less than 0 or not provided at all ||
|| `-`     | `The parameter 'fields' must be array` | The `fields` is not an object ||
|| `ACCESS_DENIED` | `Access denied!` | The user does not have permission to edit contacts ||
|| `-`     | `Not found` | Contact with the provided `id` not found ||
|| `-`     | `The parameter 'fields' is not valid` | Can occur for several reasons:
- if the required parameter `fields.COMPANY_ID` is not provided
- if the provided parameter `fields.COMPANY_ID` is less than or equal to 0 ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-contact-company-delete.md)
- [{#T}](./crm-contact-company-fields.md)
- [{#T}](./crm-contact-company-items-get.md)
- [{#T}](./crm-contact-company-items-set.md)
- [{#T}](./crm-contact-company-items-delete.md)

[1]: ../../../data-types.md
[2]: ../../data-types.md