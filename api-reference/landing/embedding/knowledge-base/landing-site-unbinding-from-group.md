# Unbind Knowledge Base from Social Network Group landing.site.unbindingFromGroup

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`landing`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with View permission in the Sites section and editing rights for the Knowledge Base in the specified group.

The method `landing.site.unbindingFromGroup` removes the binding of the Knowledge Base to a Social Network group.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id**^*^
[`integer`](../../../data-types.md) | Identifier of the Knowledge Base site.

`id` can be obtained, for example, from the method [landing.site.getList](../../site/landing-site-get-list.md) in the `ID` field ||
|| **groupId**^*^
[`integer`](../../../data-types.md) | Identifier of the Social Network group.

`groupId` can be obtained from:
- the group interface
- the result of the method [landing.site.getGroupBindings](./landing-site-get-group-bindings.md) in the `BINDING_ID` field
- the method [socialnetwork.api.workgroup.list](../../../sonet-group/socialnetwork-api-workgroup-list.md)
- the method [sonet_group.get](../../../sonet-group/sonet-group-get.md) ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

Example of unbinding the Knowledge Base from a group, where:
- `id` — identifier of the Knowledge Base site
- `groupId` — identifier of the group

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "id": 32,
        "groupId": 174
      }' \
      "https://**put.your-domain-here**/rest/**user_id**/**webhook_code**/landing.site.unbindingFromGroup.json"
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "id": 32,
        "groupId": 174,
        "auth": "**put_access_token_here**"
      }' \
      "https://**put.your-domain-here**/rest/landing.site.unbindingFromGroup.json"
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'landing.site.unbindingFromGroup',
    		{
    			id: 32,
    			groupId: 174
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
                'landing.site.unbindingFromGroup',
                [
                    'id' => 32,
                    'groupId' => 174,
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . var_export($result, true);
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error unbinding site from group: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'landing.site.unbindingFromGroup',
        {
            id: 32,
            groupId: 174
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
    require_once('crest.php');

    $result = CRest::call(
        'landing.site.unbindingFromGroup',
        [
            'id' => 32,
            'groupId' => 174,
        ]
    );

    if (isset($result['error']))
    {
        echo 'Error: ' . $result['error_description'];
    }
    else
    {
        echo '<pre>';
        print_r($result['result']);
        echo '</pre>';
    }
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": true,
    "time": {
        "start": 1774952664,
        "finish": 1774952665.017161,
        "duration": 1.0171608924865723,
        "processing": 0,
        "date_start": "2026-03-31T13:24:24+03:00",
        "date_finish": "2026-03-31T13:24:25+03:00",
        "operating_reset_at": 1774953265,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../../data-types.md) | Result of the unbinding:

- `true` — binding removed
- `false` — binding not removed ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the execution time of the request ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "ACCESS_DENIED",
    "error_description": "Insufficient permissions."
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `MISSING_PARAMS` | Insufficient call parameters, missing: groupId | Method call without `groupId` ||
|| `MISSING_PARAMS` | Insufficient call parameters, missing: id | Method call without `id` ||
|| `TYPE_ERROR` | Bitrix\Landing\PublicAction\Site::unbindingFromGroup(): Argument #2 ($groupId) must be of type int, string given | Parameter `groupId` passed as a string instead of `int` ||
|| `TYPE_ERROR` | Bitrix\Landing\PublicAction\Site::unbindingFromGroup(): Argument #1 ($id) must be of type int, string given | Parameter `id` passed as a string instead of `int` ||
|| `ACCESS_DENIED` | Insufficient permissions | User did not pass general access checks ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./landing-site-binding-to-group.md)
- [{#T}](./landing-site-get-group-bindings.md)
- [{#T}](./landing-site-unbinding-from-menu.md)
- [{#T}](./landing-site-get-menu-bindings.md)
- [{#T}](./index.md)