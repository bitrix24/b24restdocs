# Bind to Social Network Group landing.site.bindingToGroup

> Scope: [`landing`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with the Posting in Extensions permission in the Knowledge Base section and editing rights for the Knowledge Base in the specified group.

The method `landing.site.bindingToGroup` binds the Knowledge Base to a Social Network group.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id**^*^
[`integer`](../../../data-types.md) | Identifier of the Knowledge Base site.

`id` can be obtained, for example, from the [landing.site.getList](../../site/landing-site-get-list.md) method in the `ID` field ||
|| **groupId**^*^
[`integer`](../../../data-types.md) | Identifier of the Social Network group.

`groupId` can be obtained from:
- the group interface
- the result of the [landing.site.getGroupBindings](./landing-site-get-group-bindings.md) method in the `BINDING_ID` field for existing bindings
- the [socialnetwork.api.workgroup.list](../../../sonet-group/socialnetwork-api-workgroup-list.md) method
- the [sonet_group.get](../../../sonet-group/sonet-group-get.md) method ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

Example of binding the Knowledge Base to a group, where:
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
      "https://**put.your-domain-here**/rest/**user_id**/**webhook_code**/landing.site.bindingToGroup.json"
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
      "https://**put.your-domain-here**/rest/landing.site.bindingToGroup.json"
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'landing.site.bindingToGroup',
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
                'landing.site.bindingToGroup',
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
        echo 'Error binding site to group: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'landing.site.bindingToGroup',
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
        'landing.site.bindingToGroup',
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

HTTP status: **200**

```json
{
    "result": true,
    "time": {
        "start": 1774952664,
        "finish": 1774952665.017161,
        "duration": 1.0171608924865723,
        "processing": 0,
        "date_start": "2026-03-31T13:24:24+02:00",
        "date_finish": "2026-03-31T13:24:25+02:00",
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
[`boolean`](../../../data-types.md) | Result of the binding:

- `true` — binding completed
- `false` — binding not completed

The method may return `false` without an error if:
- the user does not have the Posting in Extensions permission in the Knowledge Base section
- the user does not have editing rights for the Knowledge Base in the specified group
- a binding for the Knowledge Base already exists for the group ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

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
|| `MISSING_PARAMS` | Not enough call parameters, missing: groupId | Method call without `groupId` ||
|| `MISSING_PARAMS` | Not enough call parameters, missing: id | Method call without `id` ||
|| `TYPE_ERROR` | Bitrix\Landing\PublicAction\Site::bindingToGroup(): Argument #2 ($groupId) must be of type int, string given | Parameter `groupId` passed as a string instead of `int` ||
|| `TYPE_ERROR` | Bitrix\Landing\PublicAction\Site::bindingToGroup(): Argument #1 ($id) must be of type int, string given | Parameter `id` passed as a string instead of `int` ||
|| `ACCESS_DENIED` | Insufficient permissions | User did not pass general access checks ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./landing-site-unbinding-from-group.md)
- [{#T}](./landing-site-get-group-bindings.md)
- [{#T}](./landing-site-binding-to-menu.md)
- [{#T}](./landing-site-get-menu-bindings.md)
- [{#T}](./index.md)