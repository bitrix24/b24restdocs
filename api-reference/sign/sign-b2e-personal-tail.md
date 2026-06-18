# Get a list of signed documents for the user sign.b2e.personal.tail

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`sign.b2e`](../scopes/permissions.md)
>
> Who can execute the method: a user with access to their signed documents

The method `sign.b2e.personal.tail` returns a list of signed documents for the user from the e-Signature for HR section.

The method works only in the context of authorization of the [application](../../settings/app-installation/index.md).

## Method Parameters

{% include [Note on required parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **limit**
[`integer`](../data-types.md) | Number of records per page. 

The parameter accepts a value from 1 to 50. 

By default, 20 records are displayed per page ||
|| **offset**
[`integer`](../data-types.md) | This parameter is used for pagination control. It is similar to the standard parameter [start](../../settings/performance/huge-data.md).
 
The page size of results depends on the **limit** parameter
||
|#

## Code Examples

{% include [Note on examples](../../_includes/examples.md) %}

{% list tabs %}

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"limit":2,"offset":0,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sign.b2e.personal.tail
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame, ISODate } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of each signed document returned in result[]
    type SignedDocumentItem = {
      id: number
      title: string
      signed_date: ISODate | null
      file_url: string
    }

    try {
      const response = await $b24.actions.v2.call.make<SignedDocumentItem[]>({
        method: 'sign.b2e.personal.tail',
        params: {
          // Number of records per page. Value from 1 to 50. Default is 20.
          limit: 2,
          // Offset for pagination (analogous to the standard start parameter).
          offset: 0,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Signed documents:', result.length, result)
      }
    } catch (error) {
      // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
      console.error(error)
    }
    ```

- JS (UMD)

    ```html
    <!-- Load the SDK (UMD build); it is exposed as the global B24Js -->
    <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
    <script>
      async function fetchSignedDocuments() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'sign.b2e.personal.tail',
            params: {
              // Number of records per page. Value from 1 to 50. Default is 20.
              limit: 2,
              // Offset for pagination (analogous to the standard start parameter).
              offset: 0,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Signed documents:', result.length, result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', fetchSignedDocuments)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'sign.b2e.personal.tail',
                [
                    'limit'  => 2,
                    'offset' => 0
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
        // Your logic for processing data
        processData($result);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```javascript
    BX24.callMethod(
        'sign.b2e.personal.tail',
        {
            // Number of records per page. Value from 1 to 50. Default is 20.
            limit: 2,
            
            // Parameter for pagination control.
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

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'sign.b2e.personal.tail',
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

HTTP status: **200**

```json
{
    "result": [
        {
            "id": 59,
            "title": "test-pdf e",
            "signed_date": "2024-06-28T19:34:58+02:00",
            "file_url": "https://your-domain.bitrix24.com/rest/download.json?auth=6348b3670000071b0075444600000001f0f1073855cfba3bff42f043e2c1c26a46cb93&token=sign.b2e%7CaWQ9NTkmXz1udzlucFJBVHUxM2JjcUV2YncyY0tQbTZNSTNzT0Z3MA%3D%3D%7CImRvd25sb2FkfHNpZ24uYjJlfGFXUTlOVGttWHoxdWR6bHVjRkpCVkhVeE0ySmpjVVYyWW5jeVkwdFFiVFpOU1ROelQwWjNNQT09fDYzNDhiMzY3MDAwMDA3MWIwMDc1NDQ0NjAwMDAwMDAxZjBmMTA3Mzg1NWNmYmEzYmZmNDJmMDQzZTJjMWMyNmE0NmNiOTMi.AoYFUXxsuvEjW9ipqBndwej6EvcjBWJTXMh9QQ3O6BU%3D"
        },
        {
            "id": 55,
            "title": "test-pdf 778484",
            "signed_date": "2024-06-28T18:39:47+02:00",
            "file_url": "https://your-domain.bitrix24.com/rest/download.json?auth=6348b3670000071b0075444600000001f0f1073855cfba3bff42f043e2c1c26a46cb93&token=sign.b2e%7CaWQ9NTUmXz04eDU2VkhCUU9hZ0xQQzA3eDJLNWRuYmJ4dTFYOWgzOA%3D%3D%7CImRvd25sb2FkfHNpZ24uYjJlfGFXUTlOVFVtWHowNGVEVTJWa2hDVVU5aFoweFFRekEzZURKTE5XUnVZbUo0ZFRGWU9XZ3pPQT09fDYzNDhiMzY3MDAwMDA3MWIwMDc1NDQ0NjAwMDAwMDAxZjBmMTA3Mzg1NWNmYmEzYmZmNDJmMDQzZTJjMWMyNmE0NmNiOTMi.PYj60eOODc0X4n0pbwMFwIJKV3uZTlSpZBGCmPaj%2F7A%3D"
        }
    ],
    "time": {
        "start": 1739799244.3062601,
        "finish": 1739799244.3473959,
        "duration": 0.04113578796386719,
        "processing": 0.01007699966430664,
        "date_start": "2025-02-17T16:34:04+02:00",
        "date_finish": "2025-02-17T16:34:04+02:00",
        "operating_reset_at": 1739799844,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../data-types.md) | The root element of the response. Contains information about the user's signed documents ||
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
|| **signed_date**
[`string`](../data-types.md) | Date of signing the document ||
|| **file_url**
[`string`](../data-types.md) | Link to download the signed document ||
|#

## Error Handling

HTTP status: **401**

```json
{
    "error": "WRONG_AUTH_TYPE",
    "error_description": "Current authorization type is denied for this method Application context required"
}
```

{% include notitle [error handling](../../_includes/error-info.md) %}

### Possible Error Codes

{% include [system errors](../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./index.md)
- [{#T}](./sign-b2e-mysafe-tail.md)
