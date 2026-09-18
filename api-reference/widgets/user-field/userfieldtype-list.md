# Get a list of registered user field types userfieldtype.list

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`placement`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

The method `userfieldtype.list` returns the user field types that the application registered with the method [userfieldtype.add](./userfieldtype-add.md). For each type, the method returns the code, the handler address, the name, and the description.

A field of such a type is displayed in the CRM item card, and its content is loaded by the application handler. The general workflow is described in the article [Custom Field Types](./index.md).

Use the method to retrieve the type code `USER_TYPE_ID` before calling [userfieldtype.update](./userfieldtype-update.md) or [userfieldtype.delete](./userfieldtype-delete.md).

{% note info "" %}

The method works only in the context of an [application](../../../settings/app-installation/index.md)

{% endnote %}

## Method Parameters

#|
|| **Name**
`type` | **Description** ||
|| **start**
[`integer`](../../data-types.md) | Offset for pagination. The method returns no more than 50 records per call. To retrieve the next page, pass the `next` value from the previous response: `50` for the second page, `100` for the third. For details, see the article [Features of List Methods](../../../settings/how-to-call-rest-api/list-methods-pecularities.md) ||
|#

## Code Examples

{% include [Footnote on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (OAuth)

    ```curl
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"start":0,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/userfieldtype.list
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of each UserFieldTypeItem returned in result[]
    type UserFieldTypeItem = {
      USER_TYPE_ID: string
      HANDLER: string
      TITLE: string
      DESCRIPTION: string
    }

    // userfieldtype.list returns a single page (max 50 records). For the whole result set
    // use a list helper: $b24.actions.v2.callList.make() returns every record as one
    // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
    try {
      const response = await $b24.actions.v2.call.make<UserFieldTypeItem[]>({
        method: 'userfieldtype.list',
        params: {
          start: 0,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('User field types count:', result.length, result)
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
      async function listUserFieldTypes() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          // userfieldtype.list returns a single page (max 50 records). For the whole result set
          // use a list helper: $b24.actions.v2.callList.make() returns every record as one
          // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
          const response = await $b24.actions.v2.call.make({
            method: 'userfieldtype.list',
            params: {
              start: 0,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('User field types count:', result.length, result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', listUserFieldTypes)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.userfieldtype.list(
            start=0,
        ).response
        result = bitrix_response.result
        print(result)
    except BitrixAPIError as error:
        print(
            "Bitrix API error",
            f"error: {error.error}",
            f"error_description: {error.error_description}",
            sep="\n",
        )
    except BitrixSDKException as error:
        print(f"Bitrix SDK error: {error.message}")
    except Exception as error:
        print(f"Unexpected error: {error}")
    ```

- PHP

    ```php
    try {
        $userFieldTypesResult = $serviceBuilder->getPlacementScope()->userFieldType()->list();
        $userFieldTypes = $userFieldTypesResult->getUserFieldTypes();
        foreach ($userFieldTypes as $userFieldType) {
            print("Description: " . $userFieldType->DESCRIPTION . "\n");
            print("Handler: " . $userFieldType->HANDLER . "\n");
            print("Title: " . $userFieldType->TITLE . "\n");
            print("User Type ID: " . $userFieldType->USER_TYPE_ID . "\n");
        }
    } catch (Throwable $e) {
        print("Error: " . $e->getMessage());
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'userfieldtype.list',
        { start: 0 },
        function(result)
        {
            if(result.error())
                console.error(result.error());
            else
                console.log(result.data());
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'userfieldtype.list',
        ['start' => 0]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "userfieldtype.list", nil, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("userfieldtype.list: %w", err)
    }

    var items []struct {
    	UserTypeID  string `json:"USER_TYPE_ID"`
    	Handler     string `json:"HANDLER"`
    	Title       string `json:"TITLE"`
    	Description string `json:"DESCRIPTION"`
    }
    if err := json.Unmarshal(res.Result, &items); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    for _, it := range items {
    	fmt.Println(it.UserTypeID, it.Handler)
    }

    // Total and Next are filled in by list methods; for a full
    // list traversal, use client.Core().Pages and Scan.
    if res.Total != nil {
    	fmt.Println("total:", *res.Total)
    }
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": [
        {
            "USER_TYPE_ID": "my_custom_type_2",
            "HANDLER": "http:\/\/test.com\/test2.php",
            "TITLE": "test title 2",
            "DESCRIPTION":"test desc 2"
        },
        {
            "USER_TYPE_ID": "my_custom_type_1",
            "HANDLER": "http:\/\/test.com\/test1.php",
            "TITLE": "test title 1",
            "DESCRIPTION": "test desc 1"
        },
        {
            "USER_TYPE_ID": "test_user_type",
            "HANDLER": "http:\/\/test.com\/test.php",
            "TITLE": "test title",
            "DESCRIPTION": "test desc"
        }
    ],
    "total": 3,
    "time": {
        "start": 1724423274.842117,
        "finish": 1724423275.558021,
        "duration": 0.7159039974212646,
        "processing": 0.0018908977508544922,
        "date_start": "2024-08-23T16:27:54+02:00",
        "date_finish": "2024-08-23T16:27:55+02:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`array`](../../data-types.md) | List of registered user field types [(detailed description)](#result). If the application has not registered any types, the list is empty ||
|| **next**
[`integer`](../../data-types.md) | Offset for the next page. Pass the value in the `start` parameter to retrieve the next 50 records. The key is present in the response only if there are records left ||
|| **total**
[`integer`](../../data-types.md) | Total number of types registered by the application ||
|| **time**
[`time`](../../data-types.md) | Information about the execution time of the request ||
|#

#### Element of the result Array {#result}

The field values are set by the application when registering the type with the method [userfieldtype.add](./userfieldtype-add.md).

#|
|| **Name**
`type` | **Description** ||
|| **USER_TYPE_ID**
[`string`](../../data-types.md) | Short type code passed at registration. It is accepted by [userfieldtype.update](./userfieldtype-update.md) and [userfieldtype.delete](./userfieldtype-delete.md). The full type code for creating a field is built as `rest_<APP_ID>_<USER_TYPE_ID>`, as described in the article [Custom Field Types](./index.md) ||
|| **HANDLER**
[`string`](../../data-types.md) | Address of the type handler. Bitrix24 loads this address in a frame inside the field ||
|| **TITLE**
[`string`](../../data-types.md) | Name of the type in the administrative interface for configuring user fields ||
|| **DESCRIPTION**
[`string`](../../data-types.md) | Description of the type in the administrative interface for configuring user fields ||
|#

## Error Handling

HTTP Status: **403**

```json
{
    "error": "WRONG_AUTH_TYPE",
    "error_description": "Current authorization type is denied for this method Application context required"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Meaning** ||
|| `403` | `WRONG_AUTH_TYPE` | Current authorization type is denied for this method Application context required | The method was called outside the application context, for example via a webhook ||
|| `403` | `ACCESS_DENIED` | Access denied! | The method was called by a user without administrator permissions ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./userfieldtype-add.md)
- [{#T}](./userfieldtype-update.md)
- [{#T}](./userfieldtype-delete.md)

