# Get a List of Signed Documents in the Company's Safe sign.b2e.mysafe.tail

> Scope: [`sign.b2e`](../scopes/permissions.md)
>
> Who can execute the method: a user with access to the "Company Safe" section. Available documents depend on the "Access to Safe Documents" permission level.

The method `sign.b2e.mysafe.tail` returns a list of signed documents in the company's safe.

The method works only in the context of [application](../app-installation/index.md) authorization.

## Method Parameters

{% include [Note on Required Parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **limit**
[`integer`](../data-types.md) | Number of records per page.

The parameter accepts a value from 1 to 50.

By default, 20 records are displayed per page ||
|| **offset**
[`integer`](../data-types.md) | This parameter is used to manage pagination. It is similar to the standard [start](../performance/huge-data.md) parameter.

The page size of results depends on the **limit** parameter
||
|#

## Code Examples

{% include [Note on Examples](../../_includes/examples.md) %}

{% list tabs %}

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"limit":2,"offset":0,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sign.b2e.mysafe.tail
    ```

- JS

    ```javascript
    BX24.callMethod(
        'sign.b2e.mysafe.tail',
        {
            // Number of records per page. Value from 1 to 50. Default is 20.
            limit: 2,
            
            // Parameter for managing pagination.
            // Used to specify the offset from the start of the list.
            offset: 0
        },
        result => {
            if (result.error()) {
                console.error(result.error());
            } else {
                console.dir(result.data());
            }
        }
    );
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'sign.b2e.mysafe.tail',
        [
            'limit' => 2, // Number of records per page
            'offset' => 0 // Offset from the start of the list
        ]
    );

    if (isset($result['error'])) {
        echo 'Error: ' . $result['error_description'];
    } else {
        echo '<PRE>';
        print_r($result['result']);
        echo '</PRE>';
    }
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": [
        {
            "id": 59,
            "title": "test-pdf e",
            "create_date": "2024-06-28T19:07:03+02:00",
            "signed_date": "2024-06-28T19:34:58+02:00",
            "creator_id": 1,
            "member_id": 1,
            "role": "signer",
            "file_url": "https://your-domain.bitrix24.com/rest/download.json?auth=7e34b4670000071b0075444600000037f0f1072e5aa442013dece15a3df95d26ed4873&token=sign.b2e%7CaWQ9NTkmXz1IVEVndlJnZUttZUFkeERtaVBRbkhwZkhhTEJFZklpYQ%3D%3D%7CImRvd25sb2FkfHNpZ24uYjJlfGFXUTlOVGttWHoxSVZFVm5kbEpuWlV0dFpVRmtlRVJ0YVZCUmJraHdaa2hoVEVKRlprbHBZUT09fDdlMzRiNDY3MDAwMDA3MWIwMDc1NDQ0NjAwMDAwMDM3ZjBmMTA3MmU1YWE0NDIwMTNkZWNlMTVhM2RmOTVkMjZlZDQ4NzMi.8C%2B3HpNFR5C0YkzTeVL%2FdhE6QJYN66CGoDzZG4VeR4Q%3D"
        },
        {
            "id": 55,
            "title": "test-pdf 778484",
            "create_date": "2024-06-28T18:37:37+02:00",
            "signed_date": "2024-06-28T18:39:47+02:00",
            "creator_id": 19,
            "member_id": 1,
            "role": "signer",
            "file_url": "https://your-domain.bitrix24.com/rest/download.json?auth=7e34b4670000071b0075444600000037f0f1072e5aa442013dece15a3df95d26ed4873&token=sign.b2e%7CaWQ9NTUmXz12czNjZDhyM3g2SUZYdzByRVZBbVJIYzZTY3dxZUFxbw%3D%3D%7CImRvd25sb2FkfHNpZ24uYjJlfGFXUTlOVFVtWHoxMmN6TmpaRGh5TTNnMlNVWllkekJ5UlZaQmJWSklZelpUWTNkeFpVRnhidz09fDdlMzRiNDY3MDAwMDA3MWIwMDc1NDQ0NjAwMDAwMDM3ZjBmMTA3MmU1YWE0NDIwMTNkZWNlMTVhM2RmOTVkMjZlZDQ4NzMi.r6Khc2bwTlEANXvuAptaut0Z%2F6y1nGx%2FZhRKqEGkjk0%3D"
        }
    ],
    "time": {
        "start": 1739859574.5550749,
        "finish": 1739859574.595099,
        "duration": 0.0400240421295166,
        "processing": 0.0148630142211914,
        "date_start": "2025-02-18T09:19:34+02:00",
        "date_finish": "2025-02-18T09:19:34+02:00",
        "operating_reset_at": 1739860174,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../data-types.md) | Root element of the response. Contains information about signed documents in the company's safe ||
|| **time**
[`time`](../data-types.md#time) | Information about the request execution time ||
|#

#### Response Element result

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../data-types.md) | Identifier of the signed document ||
|| **title**
[`string`](../data-types.md) | Title of the document ||
|| **create_date**
[`string`](../data-types.md) | Document creation date ||
|| **signed_date**
[`string`](../data-types.md) | Document signing date ||
|| **creator_id**
[`integer`](../data-types.md) | Identifier of the user who created the document ||
|| **member_id**
[`integer`](../data-types.md) | Identifier of the user with whom the document is signed ||
|| **role**
[`string`](../data-types.md) | Employee's role in the document:                
 - editor — filler
 - reviewer — approver
 - assignee — company representative
 - signer - employee
||
|| **file_url**
[`string`](../data-types.md) | Link to download the signed document ||
|#

## Error Handling

HTTP Status: **401**

```json
{
    "error": "WRONG_AUTH_TYPE",
    "error_description": "Current authorization type is denied for this method Application context required"
}
```

{% include notitle [error handling](../../_includes/error-info.md) %}

### Possible Error Codes

{% include [system errors](../../_includes/system-errors.md) %}

## Continue Exploring

- [{#T}](./index.md)
- [{#T}](./sign-b2e-personal-tail.md)