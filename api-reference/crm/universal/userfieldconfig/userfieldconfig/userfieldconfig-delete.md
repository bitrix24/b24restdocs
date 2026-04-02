# Delete Custom Field userfieldconfig.delete

> Scope: [`userfieldconfig`](../../../../scopes/permissions.md), module scope from `moduleId` (for example, [`crm`](../../../../scopes/permissions.md))
>
> Who can execute the method: a user with permission to modify object settings in the `moduleId` module (for `crm` — permission "Allow to modify settings")

The method `userfieldconfig.delete` removes a custom field.

## Method Parameters

{% include [Footnote on parameters](../../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **moduleId***
[`string`](../../../data-types.md) | Identifier of the module from which the field is deleted ||
|| **id***
[`integer`](../../../data-types.md) | Identifier of the custom field settings.

The identifier can be obtained using the [userfieldconfig.list](./userfieldconfig-list.md) method or when creating the field with the [userfieldconfig.add](./userfieldconfig-add.md) method ||
|#

## Code Examples

{% include [Footnote on examples](../../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"moduleId":"crm","id":6953}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/userfieldconfig.delete
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"moduleId":"crm","id":6953,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/userfieldconfig.delete
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'userfieldconfig.delete',
    		{
    			moduleId: 'crm',
    			id: 6953,
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
                'userfieldconfig.delete',
                [
                    'moduleId' => 'crm',
                    'id' => 6953,
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Result: ' . print_r($result, true);
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'userfieldconfig.delete',
        {
            moduleId: 'crm',
            id: 6953,
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
        'userfieldconfig.delete',
        [
            'moduleId' => 'crm',
            'id' => 6953,
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
    "result": null,
    "time": {
        "start": 1724239307.903115,
        "finish": 1724239308.567422,
        "duration": 0.6643068790435791,
        "processing": 0.20090818405151367,
        "date_start": "2024-08-21T13:21:47+02:00",
        "date_finish": "2024-08-21T13:21:48+02:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
`null` | Returns `null` if the field is successfully deleted ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "",
    "error_description": "You cannot delete the custom field"
}
```

{% include notitle [error handling](../../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `-` | You cannot delete the custom field | Insufficient permissions to delete the field. This error is also returned if the field with the provided `id` has already been deleted or is unavailable in the context of `moduleId` ||
|| `-` | The current method required more scopes. (crm) | The application does not have the required scope for the module from `moduleId` ||
|| `-` | No settings for UserFieldAccess | Access to custom fields is not configured for the provided `moduleId` ||
|| `-` | Error while trying to change custom field settings | General error when deleting the field ||
|#

{% include [system errors](../../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./userfieldconfig-add.md)
- [{#T}](./userfieldconfig-update.md)
- [{#T}](./userfieldconfig-get.md)
- [{#T}](./userfieldconfig-list.md)
- [{#T}](./userfieldconfig-get-types.md)