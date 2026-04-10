# Update the entity.section.update method

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`entity`](../../scopes/permissions.md)
>
> Who can execute the method: a user with access permission level `W` (write) or `X` (manage) on the data storage

The `entity.section.update` method updates a section of the application's data storage.

{% note info "" %}

The method works only in the context of the [application](../../../settings/app-installation/index.md).

{% endnote %}


## Method Parameters

{% include [Footnote about parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **ENTITY**^*^
[`string`](../../data-types.md) | Identifier of the application's data storage. Use the value specified when creating the storage.

You can obtain the identifier using the [entity.get](../entities/entity-get.md) method. ||
|| **ID**^*^
[`integer`](../../data-types.md) | Identifier of the section to be updated. ||
|| **NAME**
[`string`](../../data-types.md) | Name of the section. ||
|| **SECTION**
[`integer`](../../data-types.md) | Identifier of the parent section. ||
|| **ACTIVE**
[`string`](../../data-types.md) | Active flag for the section:
- `Y` — section is active
- `N` — section is inactive. ||
|| **SORT**
[`integer`](../../data-types.md) | Sorting index of the section. ||
|| **CODE**
[`string`](../../data-types.md) | Symbolic code of the section. ||
|| **DESCRIPTION**
[`string`](../../data-types.md) | Description of the section. ||
|| **PICTURE**
[`file`](../../data-types.md) | Image of the section. File format — see the article [How to upload files](../../files/how-to-upload-files.md). ||
|| **DETAIL_PICTURE**
[`file`](../../data-types.md) | Detailed image of the section. File format — see the article [How to upload files](../../files/how-to-upload-files.md). ||
|| **UF_**
[`object`](../../data-types.md) | Custom fields of the section `UF_*` in the format `{"UF_CODE": value}` ||
|#

## Code Examples

{% include [Footnote about examples](../../../_includes/examples.md) %}

Example of updating a section where:
- `ENTITY` — identifier of the storage `dish`
- `ID` — identifier of the section `673`
- all main parameters are updated: `NAME`, `SECTION`, `ACTIVE`, `SORT`, `CODE`, `DESCRIPTION`, `PICTURE`, `DETAIL_PICTURE`, `UF_*`

{% list tabs %}

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"ENTITY":"dish","ID":673,"NAME":"Test Section (updated)","SECTION":671,"ACTIVE":"Y","SORT":550,"CODE":"test-section-updated","DESCRIPTION":"Updated description","PICTURE":["section.jpg","**base64_section_image**"],"DETAIL_PICTURE":["section-detail.jpg","**base64_section_detail_image**"],"UF_COLOR":"#ff6600","auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/entity.section.update
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'entity.section.update',
    		{
    			ENTITY: 'dish',
    			ID: 673,
    			NAME: 'Test Section (updated)',
    			SECTION: 671,
    			ACTIVE: 'Y',
    			DESCRIPTION: 'Updated description',
    			CODE: 'test-section-updated',
    			SORT: 550,
    			PICTURE: ['section.jpg', '**base64_section_image**'],
    			DETAIL_PICTURE: ['section-detail.jpg', '**base64_section_detail_image**'],
    			UF_COLOR: '#ff6600',
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
                'entity.section.update',
                [
                    'ENTITY' => 'dish',
                    'ID' => 673,
                    'NAME' => 'Test Section (updated)',
                    'SECTION' => 671,
                    'ACTIVE' => 'Y',
                    'DESCRIPTION' => 'Updated description',
                    'CODE' => 'test-section-updated',
                    'SORT' => 550,
                    'PICTURE' => ['section.jpg', '**base64_section_image**'],
                    'DETAIL_PICTURE' => ['section-detail.jpg', '**base64_section_detail_image**'],
                    'UF_COLOR' => '#ff6600',
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
        echo 'Error updating entity section: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'entity.section.update',
        {
            ENTITY: 'dish',
            ID: 673,
            NAME: 'Test Section (updated)',
            SECTION: 671,
            ACTIVE: 'Y',
            DESCRIPTION: 'Updated description',
            CODE: 'test-section-updated',
            SORT: 550,
            PICTURE: ['section.jpg', '**base64_section_image**'],
            DETAIL_PICTURE: ['section-detail.jpg', '**base64_section_detail_image**'],
            UF_COLOR: '#ff6600',
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
        'entity.section.update',
        [
            'ENTITY' => 'dish',
            'ID' => 673,
            'NAME' => 'Test Section (updated)',
            'SECTION' => 671,
            'ACTIVE' => 'Y',
            'DESCRIPTION' => 'Updated description',
            'CODE' => 'test-section-updated',
            'SORT' => 550,
            'PICTURE' => ['section.jpg', '**base64_section_image**'],
            'DETAIL_PICTURE' => ['section-detail.jpg', '**base64_section_detail_image**'],
            'UF_COLOR' => '#ff6600',
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
        "start": 1774341623,
        "finish": 1774341623.380336,
        "duration": 0.38033604621887207,
        "processing": 0,
        "date_start": "2026-03-24T11:40:23+01:00",
        "date_finish": "2026-03-24T11:40:23+01:00",
        "operating_reset_at": 1774342223,
        "operating": 0.1318378448486328
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../data-types.md) | Result of the method execution. Returns `true` for a successful update. ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time. ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "ERROR_SECTION_NOT_FOUND",
    "error_description": "Section not found"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `ERROR_ARGUMENT` | Argument 'ENTITY' is null or empty | The `ENTITY` parameter is not provided or is empty after cleaning. ||
|| `ERROR_ARGUMENT` | Entity code is too long. Max length is N characters. | The `ENTITY` value is too long. ||
|| `ERROR_ARGUMENT` | Argument 'ID' is null or empty | The `ID` parameter is not provided or has a value of `0`. ||
|| `ERROR_ENTITY_NOT_FOUND` | Entity not found | The storage with the provided `ENTITY` was not found. ||
|| `ERROR_SECTION_NOT_FOUND` | Section not found | The section with the specified `ID` was not found in this storage. ||
|| `ACCESS_DENIED` | Access denied! | Insufficient permissions to update the section. ||
|| `ERROR_CORE` | Internal error updating entity section. Try updating again. | Internal error while updating the section. ||
|| `ERROR_CORE` | Invalid file type | An unsupported file type was provided in `PICTURE` or `DETAIL_PICTURE`. ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./entity-section-add.md)
- [{#T}](./entity-section-get.md)
- [{#T}](./entity-section-delete.md)
- [{#T}](./index.md)