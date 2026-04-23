# Delete a Set of Additional Content Blocks for the Timeline Record crm.timeline.layout.blocks.delete

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: administrator

The method `crm.timeline.layout.blocks.delete` removes a set of additional content blocks for a timeline record.

Within the application, you can only delete the set of additional content blocks that was installed through this application.

## Method Parameters

{% include [Note on Required Parameters](../../../../_includes/required.md) %}

#| 
|| **Name**
`type` | **Description** ||
|| **entityTypeId*** 
[`integer`](../../../data-types.md) | Identifier of the CRM entity type to which the timeline record is linked ||
|| **entityId*** 
[`integer`](../../../data-types.md) | Identifier of the CRM entity to which the timeline record is linked ||
|| **timelineId*** 
[`integer`](../../../data-types.md) | Identifier of the timeline record ||
|#

## Code Examples

Delete a set of additional content blocks for the timeline record with `id = 8`, linked to the deal with `id = 4`:

{% include [Note on Examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"entityTypeId":2,"entityId":4,"timelineId":8}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.timeline.layout.blocks.delete
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"entityTypeId":2,"entityId":4,"timelineId":8,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.timeline.layout.blocks.delete
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'crm.timeline.layout.blocks.delete',
            {
                entityTypeId: 2, // Deal
                entityId: 4,     // Deal ID
                timelineId: 8,   // ID of the timeline record linked to this deal
            }
        );
        
        const result = response.getData().result;
        console.info(result);
    }
    catch( error )
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
                'crm.timeline.layout.blocks.delete',
                [
                    'entityTypeId' => 2, // Deal
                    'entityId'     => 4, // Deal ID
                    'timelineId'   => 8, // ID of the timeline record linked to this deal
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
        // You can add your logic for processing the result here
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error deleting timeline layout block: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'crm.timeline.layout.blocks.delete',
        {
            entityTypeId: 2, // Deal
            entityId: 4,     // Deal ID
            timelineId: 8,   // ID of the timeline record linked to this deal
        },
        (result) => {
            if (result.error()) {
                console.error(result.error());
            } else {
                console.info(result.data());
            }
        },
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');
    $result = CRest::call(
        'crm.timeline.layout.blocks.delete',
        [
            'entityTypeId' => 2,
            'entityId' => 4,
            'timelineId' => 8,
        ]
    );
    echo '';
    print_r($result);
    echo '';
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": {
        "success": true
    },
    "time": {
        "start": 1753341040.475739,
        "finish": 1753341040.582705,
        "duration": 0.10696601867675781,
        "processing": 0.04708504676818848,
        "date_start": "2025-07-24T17:57:20+00:00",
        "date_finish": "2025-07-24T17:57:20+00:00",
        "operating": 0
    }
}
```

### Returned Data

#| 
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../data-types.md) | Root element of the response. On successful execution, contains an object with the field `success`. If the set is not deleted, returns `null` ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the execution time of the request ||
|#

#### Result Object {#result}

#| 
|| **Name**
`type` | **Description** ||
|| **success**
[`boolean`](../../../data-types.md) | Result of deleting the set of additional content blocks. The field is returned on successful execution of the method and has a value of `true` ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "ERROR_WRONG_CONTEXT",
    "error_description": "Method call is only possible in the context of a REST application"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#| 
|| **Code** | **Description** ||
|| `ERROR_WRONG_CONTEXT` | Method call is only possible in the context of a REST application ||
|| `OWNER_NOT_FOUND` | The entity to which the timeline record is linked was not found ||
|| `NOT_FOUND` | The timeline record or the set of additional content blocks was not found ||
|| `ACCESS_DENIED` | Access denied ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./index.md)
- [{#T}](./crm-timeline-layout-blocks-set.md)
- [{#T}](./crm-timeline-layout-blocks-get.md)
- [{#T}](./content-blocks-test-app.md)