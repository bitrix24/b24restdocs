# Get or Modify Access Permissions for entity.rights

> Scope: [`entity`](../../scopes/permissions.md)
>
> Who can execute the method:
> - Any user can retrieve current permissions
> - A user with access level `X` (management) can modify permissions for the data storage

The `entity.rights` method returns the current set of access permissions for the application's data storage.

{% note info "" %}

The method works only in the context of an [application](../../../settings/app-installation/index.md).

{% endnote %}


## Method Parameters

{% include [Note on parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **ENTITY**^*^
[`string`](../../data-types.md) | Identifier of the application's data storage. Use the value specified when creating the storage.

You can obtain the identifier using the [entity.get](./entity-get.md) method. ||
|| **ACCESS**
[`object`](../../data-types.md) | New set of permissions in the format `{"access_code":"access_level"}`.

Examples of access codes:
- `U<id>` — user, e.g., `U1`
- `G<id>` — user group, e.g., `G2`
- `AU` — all authorized users

The method accepts standard access codes from Bitrix24. You can check the name of the code using the [access.name](../../common/system/access-name.md) method.

Supported levels:
- `R` — read
- `W` — write
- `X` — management

If a different level is provided, that permission entry will not be added.

When `ACCESS` is passed, the current user is forcibly granted the `X` permission (`U<id>`). ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

Example of modifying access permissions, where:
- `ENTITY` — storage identifier `dish`
- `ACCESS` — new set of permissions: `U1` with level `W` and `AU` with level `R`

{% list tabs %}

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"ENTITY":"dish","ACCESS":{"U1":"W","AU":"R"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/entity.rights
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'entity.rights',
    		{
    			ENTITY: 'dish',
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
                'entity.rights',
                [
                    'ENTITY' => 'dish',
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
        echo 'Error getting entity rights: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'entity.rights',
        {
            ENTITY: 'dish',
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
        'entity.rights',
        [
            'ENTITY' => 'dish',
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
    "result": {
        "U1": "W",
        "AU": "R",
        "U577": "X"
    },
    "time": {
        "start": 1774267885,
        "finish": 1774267885.803565,
        "duration": 0.8035650253295898,
        "processing": 0,
        "date_start": "2026-03-23T15:11:25+01:00",
        "date_finish": "2026-03-23T15:11:25+01:00",
        "operating_reset_at": 1774268485,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`result`](#result) | Root element of the response. Contains access permissions for the storage ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

#### Type result {#result}

#|
|| **Name**
`type` | **Description** ||
||
[`object`](../../data-types.md) | Access permissions object in the format `{"access_code":"access_level"}`, where access level is `R`, `W`, or `X` ||
||
`null` | Returned if the storage with the provided `ENTITY` is not found ||
|#

## Error Handling

HTTP Status: **400**

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
|| `ERROR_ARGUMENT` | Argument 'ENTITY' is null or empty | The `ENTITY` parameter was not provided or is empty after cleaning ||
|| `ERROR_ARGUMENT` | Entity code is too long. Max length is 13 characters. | The `ENTITY` value is too long ||
|| `ACCESS_DENIED` | Access denied! | Insufficient permissions to modify the storage access rights ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./entity-add.md)
- [{#T}](./entity-update.md)
- [{#T}](./entity-get.md)
- [{#T}](./entity-delete.md)