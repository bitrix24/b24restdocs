# Remove Contact from Specified Deal crm.deal.contact.delete

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with "modify" access permission for deals.

The method `crm.deal.contact.delete` removes a contact from the specified deal.

## Method Parameters

{% include [Note on parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../../data-types.md) | Identifier of the deal.

Can be obtained using the methods [crm.deal.list](../crm-deal-list.md) or [crm.deal.add](../crm-deal-add.md).
||
|| **fields***
[`object`](../../../data-types.md) | An object containing information about which contact needs to be removed from the bindings [(detailed description)](#parameter-fields) ||
|#

### Parameter fields {#parameter-fields}

{% include [Note on parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **CONTACT_ID***
[`crm_entity`](../../data-types.md) | Identifier of the contact to be removed from the bindings.

The identifier can be obtained using the methods [crm.contact.list](../../contacts/crm-contact-list.md) or [crm.contact.add](../../contacts/crm-contact-add.md) ||
|#

{% note info "Remove Primary Binding" %}

If the primary binding is removed, the first available binding will become the new primary.

{% endnote %}

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

Example of removing a deal-contact link, where:
- deal identifier — `1875`
- contact identifier — `55`

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":1875,"fields":{"CONTACT_ID":55}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.deal.contact.delete
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":1875,"fields":{"CONTACT_ID":55},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.deal.contact.delete
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'crm.deal.contact.delete',
    		{
    			id: 1875,
    			fields: {
    				CONTACT_ID: 55,
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
                'crm.deal.contact.delete',
                [
                    'id' => 1875,
                    'fields' => [
                        'CONTACT_ID' => 55,
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
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error deleting deal contact: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'crm.deal.contact.delete',
        {
            id: 1875,
            fields: {
                CONTACT_ID: 55,
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
        'crm.deal.contact.delete',
        [
            'id' => 1875,
            'fields' => [
                'CONTACT_ID' => 55,
            ]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Python

    Example

    ```python
    from b24pysdk.client import BaseClient
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    client: BaseClient

    try:
        bitrix_response = client.crm.deal.contact.delete(
            bitrix_id=123,
            fields={"CONTACT_ID": 456},
        ).response
        result = bitrix_response.result
        print(result)
    except BitrixAPIError as error:
        print(
            "Bitrix API Error",
            f"error: {error.error}",
            f"error_description: {error.error_description}",
            sep="\n",
        )
    except BitrixSDKException as error:
        print(f"Bitrix SDK Error: {error.message}")
    except Exception as error:
        print(f"Unexpected error: {error}")
    ```
{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": true,
    "time": {
        "start": 1773234105,
        "finish": 1773234105.847951,
        "duration": 0.8479509353637695,
        "processing": 0,
        "date_start": "2026-03-11T16:01:45+01:00",
        "date_finish": "2026-03-11T16:01:45+01:00",
        "operating_reset_at": 1773234705,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../../data-types.md) | Root element of the response. Contains:
- `true` — contact removed from the deal
- `false` — contact not linked to the deal
||
|| **time**
[`time`](../../../data-types.md#time) | Information about the execution time of the request ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "",
    "error_description": "Not found."
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `-` | `The parameter 'ownerEntityID' is invalid or not defined.` | The `id` is less than 1 or not provided at all ||
|| `-` | `The parameter 'item' must be array.` | The `fields` is not an object ||
|| `-` | `Access denied.` | The user does not have permission to modify deals ||
|| `ACCESS_DENIED` | `Access denied!` | No permission to modify the deal ||
|| `-` | `Not found.` | The deal with the provided `id` was not found ||
|| `-` | `The parameter 'fields' is not valid.` | Can occur for several reasons:
- if the required parameter `fields.CONTACT_ID` is not provided
- if the provided parameter `fields.CONTACT_ID` is less than or equal to 0 ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-deal-contact-add.md)
- [{#T}](./crm-deal-contact-fields.md)
- [{#T}](./crm-deal-contact-items-get.md)
- [{#T}](./crm-deal-contact-items-set.md)
- [{#T}](./crm-deal-contact-items-delete.md)