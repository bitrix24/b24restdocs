# Get Site Folders landing.site.getFolders

> Scope: [`landing`](../../scopes/permissions.md)
>
> Who can execute the method: any user with "view" access permission for the site

The method `landing.site.getFolders` returns a list of site folders.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **siteId***
[`integer`](../../data-types.md) | The identifier of the site for which folders are requested.

The site identifier can be obtained using the method [landing.site.getList](./landing-site-get-list.md) or from the result of the method [landing.site.add](./landing-site-add.md) ||
|| **filter**
[`object`](../../data-types.md) | Filter by folder fields [(detailed description)](#filter) ||
|#

### Filter Parameter {#filter}

#|
|| **Name**
`type` | **Description** ||
|| **ID**
[`integer`](../../data-types.md) | The identifier of the folder ||
|| **PARENT_ID**
[`integer`](../../data-types.md) \| `null` | The identifier of the parent folder. If `0`, `null`, `false`, or an empty string is passed, the value will be converted to `null`, which retrieves top-level folders ||
|| **INDEX_ID**
[`integer`](../../data-types.md) | The identifier of the folder's index page ||
|| **ACTIVE**
[`string`](../../data-types.md) | Active flag `Y/N` ||
|| **DELETED**
[`string`](../../data-types.md) | Deleted flag `Y/N` ||
|| **=DELETED**
[`string`](../../data-types.md) | Exact match for the deleted flag. If `DELETED` and `=DELETED` are not present in the request, the method automatically adds `=DELETED: "N"` ||
|| **TITLE**
[`string`](../../data-types.md) | The title of the folder ||
|| **CODE**
[`string`](../../data-types.md) | The symbolic code of the folder ||
|| **CREATED_BY_ID**
[`integer`](../../data-types.md) | The identifier of the user who created the folder ||
|| **MODIFIED_BY_ID**
[`integer`](../../data-types.md) | The identifier of the user who modified the folder ||
|| **DATE_CREATE**
[`datetime`](../../data-types.md) | The date and time the folder was created ||
|| **DATE_MODIFY**
[`datetime`](../../data-types.md) | The date and time the folder was modified ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "siteId": 1817,
        "filter": {
          "PARENT_ID": 0,
          "=DELETED": "N",
          "ACTIVE": "Y"
        }
      }' \
      "https://**put.your-domain-here**/rest/**user_id**/**webhook_code**/landing.site.getFolders.json"
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "siteId": 1817,
        "filter": {
          "PARENT_ID": 0,
          "=DELETED": "N",
          "ACTIVE": "Y"
        },
        "auth": "**put_access_token_here**"
      }' \
      "https://**put.your-domain-here**/rest/landing.site.getFolders.json"
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'landing.site.getFolders',
    		{
    			siteId: 1817,
    			filter: {
    				PARENT_ID: 0,
    				'=DELETED': 'N',
    				ACTIVE: 'Y'
    			}
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
                'landing.site.getFolders',
                [
                    'siteId' => 1817,
                    'filter' => [
                        'PARENT_ID' => 0,
                        '=DELETED' => 'N',
                        'ACTIVE' => 'Y',
                    ],
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting folders: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'landing.site.getFolders',
        {
            siteId: 1817,
            filter: {
                PARENT_ID: 0,
                '=DELETED': 'N',
                ACTIVE: 'Y'
            }
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
        'landing.site.getFolders',
        [
            'siteId' => 1817,
            'filter' => [
                'PARENT_ID' => 0,
                '=DELETED' => 'N',
                'ACTIVE' => 'Y',
            ],
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
    "result": [
        {
            "ID": "1",
            "PARENT_ID": null,
            "SITE_ID": "3",
            "INDEX_ID": "9",
            "ACTIVE": "Y",
            "DELETED": "N",
            "TITLE": "test page transfer",
            "CODE": "change",
            "CREATED_BY_ID": "1",
            "MODIFIED_BY_ID": "29",
            "DATE_CREATE": "11/12/2021 08:39:53 pm",
            "DATE_MODIFY": "12/28/2021 03:04:04 pm"
        },
        {
            "ID": "33",
            "PARENT_ID": null,
            "SITE_ID": "3",
            "INDEX_ID": null,
            "ACTIVE": "Y",
            "DELETED": "N",
            "TITLE": "mobile",
            "CODE": "mobile",
            "CREATED_BY_ID": "29",
            "MODIFIED_BY_ID": "29",
            "DATE_CREATE": "12/08/2021 10:23:37 am",
            "DATE_MODIFY": "12/08/2021 10:24:48 am"
        },
        {
            "ID": "3",
            "PARENT_ID": null,
            "SITE_ID": "3",
            "INDEX_ID": "45",
            "ACTIVE": "Y",
            "DELETED": "N",
            "TITLE": "attachment 1",
            "CODE": "attachment1",
            "CREATED_BY_ID": "1",
            "MODIFIED_BY_ID": "29",
            "DATE_CREATE": "11/12/2021 08:39:53 pm",
            "DATE_MODIFY": "12/03/2021 08:10:21 am"
        }
    ],
    "time": {
        "start": 1773257990,
        "finish": 1773257990.091689,
        "duration": 0.0916891098022461,
        "processing": 0,
        "date_start": "2026-03-11T22:39:50+01:00",
        "date_finish": "2026-03-11T22:39:50+01:00",
        "operating_reset_at": 1773258590,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object[]`](../../data-types.md) | A list of site folders [(detailed description)](#folder). The method may return `result: []` without an error if there are no folders on the site or if the user does not have "view" access permission for the specified site ||
|| **time**
[`time`](../../data-types.md#time) | Information about the execution time of the request ||
|#

#### Folder Object {#folder}

#|
|| **Name**
`type` | **Description** ||
|| **ID**
[`string`](../../data-types.md) | The identifier of the folder ||
|| **PARENT_ID**
[`string`](../../data-types.md) \| `null` | The identifier of the parent folder ||
|| **SITE_ID**
[`string`](../../data-types.md) | The identifier of the site ||
|| **INDEX_ID**
[`string`](../../data-types.md) \| `null` | The identifier of the folder's index page ||
|| **ACTIVE**
[`string`](../../data-types.md) | Active flag `Y/N` ||
|| **DELETED**
[`string`](../../data-types.md) | Deleted flag `Y/N` ||
|| **TITLE**
[`string`](../../data-types.md) | The title of the folder ||
|| **CODE**
[`string`](../../data-types.md) | The symbolic code of the folder ||
|| **CREATED_BY_ID**
[`string`](../../data-types.md) | The identifier of the user who created the folder ||
|| **MODIFIED_BY_ID**
[`string`](../../data-types.md) | The identifier of the user who modified the folder ||
|| **DATE_CREATE**
[`string`](../../data-types.md) | The date and time of creation in string format ||
|| **DATE_MODIFY**
[`string`](../../data-types.md) | The date and time of modification in string format ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "MISSING_PARAMS",
    "error_description": "Insufficient call parameters, missing: siteId"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `MISSING_PARAMS` | The required parameter `siteId` was not provided ||
|| `ACCESS_DENIED` | Insufficient general rights to call landing methods ||
|| `TYPE_ERROR` | Data type error in method call parameters ||
|| `SYSTEM_ERROR` | Internal error during method execution ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./landing-site-add-folder.md)
- [{#T}](./landing-site-update-folder.md)
- [{#T}](./landing-site-mark-folder-delete.md)
- [{#T}](./landing-site-mark-folder-undelete.md)
- [{#T}](./landing-site-publication-folder.md)
- [{#T}](./landing-site-unpublic-folder.md)