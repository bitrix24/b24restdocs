# Create a Sales Intelligence Trace: crm.tracking.trace.add

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can execute the method:
> - any user can create a trace
> - a user with permission to modify the object can link the trace

The method `crm.tracking.trace.add` creates a Sales Intelligence trace and returns its identifier.

## Method Parameters

{% include [Footnote on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **TRACE**^*^
[`string`](../../data-types.md) | JSON string containing trace data.

For the correct value, refer to the [tutorial](../../../tutorials/crm/how-to-use-analitycs/info-to-analitics.md) ||
|| **ENTITIES**
[`object[]`](../../data-types.md) | Array of objects to be linked with the trace [more details](#entities) ||
|#

### ENTITIES Parameter {#entities}

#|
|| **Name**
`type` | **Description** ||
|| **TYPE**^*^
[`string`](../../data-types.md) | Object type. Possible values:

- `COMPANY`
- `CONTACT`
- `DEAL`
- `LEAD`
- `QUOTE` ||
|| **ID**^*^
[`integer`](../../data-types.md) | Identifier of the entity.

The user must have modification rights for the specified object ||
|#

## Code Examples

{% include [Footnote on examples](../../../_includes/examples.md) %}

Example of creating a Sales Intelligence trace, where:
- `TRACE` — JSON string with trace data
- `ENTITIES` — objects linked to the trace

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "TRACE": "{\"SOURCE_ID\":\"6\",\"SOURCE_DESC\":\"Direct sale\",\"PAGES\":[{\"URL\":\"https://example.com/\",\"DATE\":\"2024-04-03T10:26:32+01:00\"}]}",
        "ENTITIES": [
          {
            "TYPE": "CONTACT",
            "ID": 3215
          },
          {
            "TYPE": "LEAD",
            "ID": 1
          }
        ]
      }' \
      "https://**put.your-domain-here**/rest/**user_id**/**webhook_code**/crm.tracking.trace.add.json"
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "TRACE": "{\"SOURCE_ID\":\"6\",\"SOURCE_DESC\":\"Direct sale\",\"PAGES\":[{\"URL\":\"https://example.com/\",\"DATE\":\"2024-04-03T10:26:32+01:00\"}]}",
        "ENTITIES": [
          {
            "TYPE": "CONTACT",
            "ID": 3215
          },
          {
            "TYPE": "LEAD",
            "ID": 1
          }
        ],
        "auth": "**put_access_token_here**"
      }' \
      "https://**put.your-domain-here**/rest/crm.tracking.trace.add.json"
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'crm.tracking.trace.add',
    		{
    			TRACE: '{"SOURCE_ID":"6","SOURCE_DESC":"Direct sale","PAGES":[{"URL":"https://example.com/","DATE":"2024-04-03T10:26:32+01:00"}]}',
    			ENTITIES: [
    				{
    					TYPE: 'CONTACT',
    					ID: 3215
    				},
    				{
    					TYPE: 'LEAD',
    					ID: 1
    				}
    			]
    		}
    	);

    	const result = response.getData().result;
    	console.info(result);
    }
    catch (error)
    {
    	console.error(error);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.tracking.trace.add',
                [
                    'TRACE' => '{"SOURCE_ID":"6","SOURCE_DESC":"Direct sale","PAGES":[{"URL":"https://example.com/","DATE":"2024-04-03T10:26:32+01:00"}]}',
                    'ENTITIES' => [
                        [
                            'TYPE' => 'CONTACT',
                            'ID' => 3215,
                        ],
                        [
                            'TYPE' => 'LEAD',
                            'ID' => 1,
                        ],
                    ],
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error adding trace: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'crm.tracking.trace.add',
        {
            TRACE: '{"SOURCE_ID":"6","SOURCE_DESC":"Direct sale","PAGES":[{"URL":"https://example.com/","DATE":"2024-04-03T10:26:32+01:00"}]}',
            ENTITIES: [
                {
                    TYPE: 'CONTACT',
                    ID: 3215
                },
                {
                    TYPE: 'LEAD',
                    ID: 1
                }
            ]
        },
        function(result)
        {
            if (result.error())
            {
                console.error(result.error());
            }
            else
            {
                console.info(result.data());
            }
        }
    );
    ```

- PHP CRest

    ```php
    $result = CRest::call(
        'crm.tracking.trace.add',
        [
            'TRACE' => '{"SOURCE_ID":"6","SOURCE_DESC":"Direct sale","PAGES":[{"URL":"https://example.com/","DATE":"2024-04-03T10:26:32+01:00"}]}',
            'ENTITIES' => [
                [
                    'TYPE' => 'CONTACT',
                    'ID' => 3215,
                ],
                [
                    'TYPE' => 'LEAD',
                    'ID' => 1,
                ],
            ],
        ]
    );

    echo '<pre>';
    print_r($result);
    echo '</pre>';
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": 341,
    "time": {
        "start": 1775117366,
        "finish": 1775117367.080829,
        "duration": 1.0808289051055908,
        "processing": 0,
        "date_start": "2026-04-02T11:09:26+01:00",
        "date_finish": "2026-04-02T11:09:27+01:00",
        "operating_reset_at": 1775117967,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`integer`](../../data-types.md) | Identifier of the created trace ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "ERROR_CORE",
    "error_description": "Parameter `TRACE` required."
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `ERROR_CORE` | Parameter `TRACE` required. | TRACE parameter is missing ||
|| `400` | `ERROR_CORE` | Can not parse JSON in parameter `TRACE`. | TRACE value is not a valid JSON string ||
|| `400` | `ERROR_CORE` | Wrong TYPE in parameter `ENTITIES`. Allowed types: COMPANY,CONTACT,DEAL,LEAD,QUOTE | Invalid TYPE provided in ENTITIES ||
|| `400` | `ERROR_CORE` | Wrong ID in parameter `ENTITIES`. | Invalid ID provided in ENTITIES ||
|| `400` | `ERROR_CORE` | You have no access to entity `<TYPE>` with ID `<ID>`. | No permission to modify the object specified in ENTITIES ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](../../../tutorials/crm/how-to-use-analitycs/info-to-analitics.md)
- [{#T}](../../../tutorials/crm/how-to-use-analitycs/use-analitics-for-add-lead.md)
- [{#T}](../../../tutorials/crm/how-to-use-analitycs/use-analitics-for-add-contact.md)