# Change Parameters of entity.update

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`entity`](../../scopes/permissions.md)
>
> Who can execute the method: a user with access permission level `X` (management) on the data storage

The `entity.update` method updates the parameters of the application's data storage.

{% note info "" %}

The method works only in the context of the [application](../../../settings/app-installation/index.md).

{% endnote %}


## Method Parameters

{% include [Note on parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **ENTITY**^*^
[`string`](../../data-types.md) | Identifier of the application's data storage. Use the value specified when creating the storage.

You can obtain the identifier using the [entity.get](./entity-get.md) method. ||
|| **NAME**
[`string`](../../data-types.md) | New name for the storage. ||
|| **ENTITY_NEW**
[`string`](../../data-types.md) | New identifier for the storage.

Used for renaming the storage code.

Only characters `a-z`, `A-Z`, `0-9`, `_` are allowed.

The maximum length is calculated dynamically using the formula:
`50 - strlen("APP_<clientId>_")`. In most cases for Bitrix24, this is 13 characters. ||
|| **ACCESS**
[`object`](../../data-types.md) | New set of access permissions in the format `{"access_code":"permission_level"}`.

Examples of access codes:
- `U<id>` — user, e.g., `U1`
- `G<id>` — user group, e.g., `G2`
- `AU` — all authorized users

The method accepts standard access codes from Bitrix24. You can check the name of the code using the [access.name](../../common/system/access-name.md) method.

Supported levels:
- `R` — read
- `W` — write
- `X` — management

If a different level is provided, that access permission will not be added.

When `ACCESS` is provided, the current user is forcibly granted permission `X` (`U<id>`). ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

Example of updating the storage, where:
- `ENTITY` — identifier of the current storage `dish`
- `NAME` — new name `Dishes v2`
- `ENTITY_NEW` — new storage code `dish_v2`
- `ACCESS` — access permissions: `U1` with level `W` and `AU` with level `R`

{% list tabs %}

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"ENTITY":"dish","NAME":"Dishes v2","ENTITY_NEW":"dish_v2","ACCESS":{"U1":"W","AU":"R"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/entity.update
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'entity.update',
    		{
    			ENTITY: 'dish',
    			NAME: 'Dishes v2',
    			ENTITY_NEW: 'dish_v2',
    			ACCESS: {
    				U1: 'W',
    				AU: 'R',
    			},
    		}
    	);

    	const result = response.getData().result;
    	console.info(result);
    }
    catch (error)
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
                'entity.update',
                [
                    'ENTITY' => 'dish',
                    'NAME' => 'Dishes v2',
                    'ENTITY_NEW' => 'dish_v2',
                    'ACCESS' => [
                        'U1' => 'W',
                        'AU' => 'R',
                    ],
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo '<pre>';
        print_r($result);
        echo '</pre>';

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error updating entity: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'entity.update',
        {
            ENTITY: 'dish',
            NAME: 'Dishes v2',
            ENTITY_NEW: 'dish_v2',
            ACCESS: {
                U1: 'W',
                AU: 'R',
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
        'entity.update',
        [
            'ENTITY' => 'dish',
            'NAME' => 'Dishes v2',
            'ENTITY_NEW' => 'dish_v2',
            'ACCESS' => [
                'U1' => 'W',
                'AU' => 'R',
            ],
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
        "start": 1774257803,
        "finish": 1774257803.550779,
        "duration": 0.5507791042327881,
        "processing": 0,
        "date_start": "2026-03-23T12:23:23+01:00",
        "date_finish": "2026-03-23T12:23:23+01:00",
        "operating_reset_at": 1774258403,
        "operating": 0.11757302284240723
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../data-types.md) | Result of the method execution. Returns `true` on successful execution. ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time. ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "ERROR_ENTITY_NOT_FOUND",
    "error_description": "Entity not found"
}
```

```json
{
    "error": "ERROR_ARGUMENT",
    "error_description": "Argument 'ENTITY' is null or empty",
    "argument": "ENTITY"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `ERROR_ENTITY_NOT_FOUND` | Entity not found | Storage with the provided `ENTITY` not found. ||
|| `ERROR_ENTITY_ALREADY_EXISTS` | Entity already exists | Storage with `ENTITY_NEW` already exists. ||
|| `ERROR_ARGUMENT` | Argument 'ENTITY' is null or empty | Parameter `ENTITY` was not provided or is empty after cleaning. ||
|| `ERROR_ARGUMENT` | Entity code is too long. Max length is N characters. | `ENTITY_NEW` value is too long. ||
|| `ACCESS_DENIED` | Access denied! | Insufficient permissions to modify the storage. ||
|| `ERROR_CORE` | Internal error updating entity. Try updating again. | Internal error while updating the storage. ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./entity-add.md)
- [{#T}](./entity-get.md)
- [{#T}](./entity-delete.md)
- [{#T}](./entity-rights.md)