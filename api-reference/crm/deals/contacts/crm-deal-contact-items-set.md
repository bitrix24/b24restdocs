# Set a set of contacts associated with the specified deal crm.deal.contact.items.set

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with "modify" access permission for deals

The method `crm.deal.contact.items.set` sets a set of contacts associated with the specified deal. Contacts not included in the provided list will be unlinked.

## Method Parameters

{% include [Parameter Note](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../../data-types.md) | Identifier of the deal

Can be obtained using the methods [crm.deal.list](../crm-deal-list.md) or [crm.deal.add](../crm-deal-add.md)
||
|| **items***
[`object[]`](../../../data-types.md) | A set of objects describing the linked contacts to the deal [(detailed description)](#deal_contact_binding) ||
|#

### Structure of the binding object {#deal_contact_binding}

{% include [Parameter Note](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **CONTACT_ID***
[`crm_entity`](../../data-types.md) | Identifier of the contact to be linked to the deal

The identifier can be obtained using the methods [crm.contact.list](../../contacts/crm-contact-list.md) or [crm.contact.add](../../contacts/crm-contact-add.md) ||
|| **IS_PRIMARY**
[`char`](../../../data-types.md) | Indicates whether the binding is primary. Possible values:
- `Y` — yes
- `N` — no

If there is no binding with `IS_PRIMARY = Y`, it will be set for the first binding in `items`.

If multiple bindings with `IS_PRIMARY = Y` are provided, the first binding with `IS_PRIMARY = Y` will be considered primary ||
|| **SORT**
[`integer`](../../../data-types.md) | Sorting index

By default `(i + 1) * 10`, where `i` is the index of the element in the `items` array, starting from 0 ||
|#

## Code Examples

{% include [Example Note](../../../../_includes/examples.md) %}

Set the following linked contacts for the deal with `id = 1875`:
- contact with `id = 55`, make it primary and set `SORT = 100`
- contact with `id = 54`, set `SORT = 200`
- contact with `id = 56`, set `SORT = 400`

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":1875,"items":[{"CONTACT_ID":55,"IS_PRIMARY":"Y","SORT":100},{"CONTACT_ID":54,"SORT":200},{"CONTACT_ID":56,"SORT":400}]}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.deal.contact.items.set
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":1875,"items":[{"CONTACT_ID":55,"IS_PRIMARY":"Y","SORT":100},{"CONTACT_ID":54,"SORT":200},{"CONTACT_ID":56,"SORT":400}],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.deal.contact.items.set
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'crm.deal.contact.items.set',
    		{
    			id: 1875,
    			items: [
    				{
    					CONTACT_ID: 55,
    					IS_PRIMARY: "Y",
    					SORT: 100,
    				},
    				{
    					CONTACT_ID: 54,
    					SORT: 200,
    				},
    				{
    					CONTACT_ID: 56,
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
                'crm.deal.contact.items.set',
                [
                    'id'    => 1875,
                    'items' => [
                        [
                            'CONTACT_ID' => 55,
                            'IS_PRIMARY' => 'Y',
                            'SORT'       => 100,
                        ],
                        [
                            'CONTACT_ID' => 54,
                            'SORT'       => 200,
                        ],
                        [
                            'CONTACT_ID' => 56,
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
            // Your required data processing logic
            processData($result->data());
        }

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error setting deal contact items: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'crm.deal.contact.items.set',
        {
            id: 1875,
            items: [
                {
                    CONTACT_ID: 55,
                    IS_PRIMARY: "Y",
                    SORT: 100,
                },
                {
                    CONTACT_ID: 54,
                    SORT: 200,
                },
                {
                    CONTACT_ID: 56,
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
        'crm.deal.contact.items.set',
        [
            'id' => 1875,
            'items' => [
                [
                    'CONTACT_ID' => 55,
                    'IS_PRIMARY' => 'Y',
                    'SORT' => 100,
                ],
                [
                    'CONTACT_ID' => 54,
                    'SORT' => 200,
                ],
                [
                    'CONTACT_ID' => 56,
                    'SORT' => 400,
                ]
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
        bitrix_response = client.crm.deal.contact.items.set(
            bitrix_id=123,
            items=[
                {"CONTACT_ID": 456, "SORT": 10, "IS_PRIMARY": "Y"},
                {"CONTACT_ID": 789, "SORT": 20, "IS_PRIMARY": "N"},
            ],
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
        "start": 1773225418,
        "finish": 1773225418.981286,
        "duration": 0.9812860488891602,
        "processing": 0,
        "date_start": "2026-03-11T13:36:58+01:00",
        "date_finish": "2026-03-11T13:36:58+01:00",
        "operating_reset_at": 1773226018,
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
    "error_description": "Not found."
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `-` | `The parameter ownerEntityID is invalid or not defined.` | The `id` is less than 1 or not provided at all ||
|| `-` | `The parameter items must be array.` | The `items` is not an array ||
|| `-` | `Access denied.` | The user does not have permission to modify deals ||
|| `ACCESS_DENIED` | `Access denied!` | No permission to modify the deal ||
|| `-` | `Not found.` | The deal with the provided `id` was not found ||
|| `ERROR_CORE` | `-` | Internal error while normalizing bindings ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-deal-contact-add.md)
- [{#T}](./crm-deal-contact-delete.md)
- [{#T}](./crm-deal-contact-fields.md)
- [{#T}](./crm-deal-contact-items-get.md)
- [{#T}](./crm-deal-contact-items-delete.md)