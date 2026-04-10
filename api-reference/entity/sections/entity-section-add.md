# Add Section to the entity.section.add Storage

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`entity`](../../scopes/permissions.md)
>
> Who can execute the method: a user with `W` (write) or `X` (manage) access permission on the data storage.

The `entity.section.add` method adds a section to the application's data storage.

{% note info "" %}

The method works only in the context of the [application](../../../settings/app-installation/index.md).

{% endnote %}


## Method Parameters

{% include [Footnote on parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **ENTITY**^*^
[`string`](../../data-types.md) | Identifier of the application's data storage. Use the value specified when creating the storage.

You can obtain the identifier using the [entity.get](../entities/entity-get.md) method. ||
|| **NAME**^*^
[`string`](../../data-types.md) | Name of the section. ||
|| **SECTION**
[`integer`](../../data-types.md) | Identifier of the parent section. ||
|| **ACTIVE**
[`string`](../../data-types.md) | Section activity flag:
- `Y` — section is active
- `N` — section is inactive ||
|| **SORT**
[`integer`](../../data-types.md) | Section sorting index. ||
|| **CODE**
[`string`](../../data-types.md) | Symbolic code of the section. ||
|| **DESCRIPTION**
[`string`](../../data-types.md) | Description of the section. ||
|| **PICTURE**
[`file`](../../data-types.md) | Section image. File format — as described in [How to upload files](../../files/how-to-upload-files.md). ||
|| **DETAIL_PICTURE**
[`file`](../../data-types.md) | Detailed image of the section. File format — as described in [How to upload files](../../files/how-to-upload-files.md). ||
|| **UF_**
[`object`](../../data-types.md) | Custom fields of the section `UF_*` in the format `{"UF_CODE": value}` ||
|#

## Code Examples

{% include [Footnote on examples](../../../_includes/examples.md) %}

Example of adding a section to the `dish` storage:
- `ENTITY` — storage identifier
- `NAME` — section name
- `SECTION` — parent section `671`
- `ACTIVE`, `SORT`, `CODE`, `DESCRIPTION` — main parameters
- `PICTURE`, `DETAIL_PICTURE` — files in the format described in [How to upload files](../../files/how-to-upload-files.md)

{% list tabs %}

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"ENTITY":"dish","NAME":"Test Section","SECTION":671,"ACTIVE":"Y","SORT":500,"CODE":"test-section","DESCRIPTION":"Description of the test section","PICTURE":["section.jpg","**base64_section_image**"],"DETAIL_PICTURE":["section-detail.jpg","**base64_section_detail_image**"],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/entity.section.add
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'entity.section.add',
    		{
    			ENTITY: 'dish',
    			NAME: 'Test Section',
    			SECTION: 671,
    			ACTIVE: 'Y',
    			SORT: 500,
    			CODE: 'test-section',
    			DESCRIPTION: 'Description of the test section',
    			PICTURE: ['section.jpg', '**base64_section_image**'],
    			DETAIL_PICTURE: ['section-detail.jpg', '**base64_section_detail_image**'],
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
                'entity.section.add',
                [
                    'ENTITY' => 'dish',
                    'NAME' => 'Test Section',
                    'SECTION' => 671,
                    'ACTIVE' => 'Y',
                    'SORT' => 500,
                    'CODE' => 'test-section',
                    'DESCRIPTION' => 'Description of the test section',
                    'PICTURE' => ['section.jpg', '**base64_section_image**'],
                    'DETAIL_PICTURE' => ['section-detail.jpg', '**base64_section_detail_image**'],
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
        echo 'Error adding entity section: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'entity.section.add',
        {
            ENTITY: 'dish',
            NAME: 'Test Section',
            SECTION: 671,
            ACTIVE: 'Y',
            SORT: 500,
            CODE: 'test-section',
            DESCRIPTION: 'Description of the test section',
            PICTURE: ['section.jpg', '**base64_section_image**'],
            DETAIL_PICTURE: ['section-detail.jpg', '**base64_section_detail_image**'],
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
        'entity.section.add',
        [
            'ENTITY' => 'dish',
            'NAME' => 'Test Section',
            'SECTION' => 671,
            'ACTIVE' => 'Y',
            'SORT' => 500,
            'CODE' => 'test-section',
            'DESCRIPTION' => 'Description of the test section',
            'PICTURE' => ['section.jpg', '**base64_section_image**'],
            'DETAIL_PICTURE' => ['section-detail.jpg', '**base64_section_detail_image**'],
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
    "result": 673,
    "time": {
        "start": 1774275397,
        "finish": 1774275397.576672,
        "duration": 0.5766720771789551,
        "processing": 0,
        "date_start": "2026-03-23T17:16:37+01:00",
        "date_finish": "2026-03-23T17:16:37+01:00",
        "operating_reset_at": 1774275997,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`integer`](../../data-types.md) | Identifier of the created section. ||
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

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `ERROR_ARGUMENT` | Argument 'ENTITY' is null or empty | Parameter `ENTITY` is not provided or is empty after cleaning. ||
|| `ERROR_ARGUMENT` | Entity code is too long. Max length is N characters. | The value of `ENTITY` is too long. ||
|| `ERROR_ENTITY_NOT_FOUND` | Entity not found | Storage with the provided `ENTITY` was not found. ||
|| `ACCESS_DENIED` | Access denied! | Insufficient rights to create a section. ||
|| `ERROR_CORE` | Internal error adding entity section. Try adding again. | Internal error while creating the section. ||
|| `ERROR_CORE` | Invalid parent section! | An invalid `SECTION` was provided. ||
|| `ERROR_CORE` | The section block code does not match the parent section block code! | The parent section belongs to another storage. ||
|| `ERROR_CORE` | Invalid file type | A file of unsupported type was provided in `PICTURE` or `DETAIL_PICTURE`. ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./entity-section-update.md)
- [{#T}](./entity-section-get.md)
- [{#T}](./entity-section-delete.md)
- [{#T}](./index.md)