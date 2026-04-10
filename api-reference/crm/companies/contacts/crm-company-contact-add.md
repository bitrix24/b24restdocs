# Add Contact to the Specified Company crm.company.contact.add

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: user with "Edit" access permission for companies

The method `crm.company.contact.add` adds a contact to the specified company.

## Method Parameters

{% include [Note on parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../../data-types.md) | Identifier of the company.

This can be obtained using the methods [crm.company.list](../crm-company-list.md) or [crm.company.add](../crm-company-add.md)
||
|| **fields***
[`object`](../../../data-types.md) | Object in the following format:

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

The list of available fields is described [below](#parameter-fields) ||
|#

### Parameter fields {#parameter-fields}

{% include [Note on parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **CONTACT_ID***
[`crm_entity`](../../data-types.md) | Identifier of the contact to be linked to the company.

The identifier can be obtained using the method [crm.item.list](../../universal/crm-item-list.md) with `entityTypeId = 3` ||
|| **IS_PRIMARY**
[`char`](../../../data-types.md#char) | Indicates whether the link is primary. Possible values:
- `Y` — yes
- `N` — no

For the first added element, `IS_PRIMARY` defaults to `Y`.

Passing `IS_PRIMARY = Y` for a new and non-first link overrides the existing primary link ||
|| **SORT**
[`integer`](../../../data-types.md) | Sort index.

Defaults to `i + 10`, where `i` is the maximum sort index of existing links for the current company or `0` if none exist ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":32,"fields":{"CONTACT_ID":54,"IS_PRIMARY":"Y","SORT":1000}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.company.contact.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":32,"fields":{"CONTACT_ID":54,"IS_PRIMARY":"Y","SORT":1000},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.company.contact.add
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'crm.company.contact.add',
    		{
    			id: 32,
    			fields: {
    				CONTACT_ID: 54,
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
                'crm.company.contact.add',
                [
                    'id' => 32,
                    'fields' => [
                        'CONTACT_ID' => 54,
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
        echo 'Error adding company contact: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'crm.company.contact.add',
        {
            id: 32,
            fields: {
                CONTACT_ID: 54,
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
        'crm.company.contact.add',
        [
            'id' => 32,
            'fields' => [
                'CONTACT_ID' => 54,
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

HTTP Status: **200**

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
[`boolean`](../../../data-types.md) | Root element of the response. Contains:
- `true` — on success
- `false` — on failure, if the contact you are trying to add already exists in the links
||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

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
|| `-`     | `The parameter 'ownerEntityID' is invalid or not defined` | The provided `id` is less than or equal to 0 or not provided at all ||
|| `-`     | `The parameter 'fields' must be array` | The `fields` parameter is not an object ||
|| `ACCESS_DENIED` | `Access denied!` | The user does not have permission to edit the company ||
|| `-`     | `Not found` | The company with the provided `id` was not found ||
|| `-`     | `The parameter 'fields' is not valid` | Can occur for several reasons:
- if the required parameter `fields.CONTACT_ID` is not provided
- if the provided parameter `fields.CONTACT_ID` is less than or equal to 0 ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-company-contact-delete.md)
- [{#T}](./crm-company-contact-fields.md)
- [{#T}](./crm-company-contact-items-get.md)
- [{#T}](./crm-company-contact-items-set.md)
- [{#T}](./crm-company-contact-items-delete.md)