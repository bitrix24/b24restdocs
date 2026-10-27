# Update Inventory Document Custom Field Values v 3.0 catalog.document.userfield.update

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: a user with the "Create and edit" and "View" permissions for documents of this type and the "View product catalog" and "View Inventory Management area" permissions

{% note info "" %}

The method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../rest-v3.md).

{% endnote %}

The method `catalog.document.userfield.update` updates the custom field values of an inventory document and returns them.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../data-types.md) | Inventory document identifier.

The identifier can be retrieved using the [catalog.document.list](../document/catalog-document-list-rest-v3.md) method ||
|| **fields***
[`object`](../../data-types.md) | Object with the document custom fields to update. Pass only the fields being changed. [Field description](#fields) ||
|#

### Parameter fields {#fields}

#|
|| **Name**
`type` | **Description** ||
|| **docType***
[`string`](../../data-types.md) | Document type. Must match the type of the document `id`, otherwise the method returns an error ||
|| **userFields**
[`object`](../../data-types.md) | Custom field values. The key is `FIELD_{id}`, where `{id}` is the custom field identifier, or a field code of the form `UF_*`.

Custom field identifiers and codes can be retrieved using the [userfieldconfig.list](../../crm/universal/userfieldconfig/userfieldconfig-list.md) method.

Keys that documents of this type do not have are skipped by the method without an error ||
|#

The method also updates the values of a processed document.

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/catalog.document.userfield.update`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":325,"fields":{"docType":"A","userFields":{"FIELD_7097":"1"}}}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/catalog.document.userfield.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":325,"fields":{"docType":"A","userFields":{"FIELD_7097":"1"}},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/catalog.document.userfield.update
    ```

- JS (TS)

    ```ts
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type DocumentUserfieldUpdateResult = {
      item: {
        id: number
        docType: string
        userFields: Record<string, unknown>
      }
    }

    try {
      const response = await $b24.actions.v3.call.make<DocumentUserfieldUpdateResult>({
        method: 'catalog.document.userfield.update',
        params: {
          id: 325,
          fields: {
            docType: 'A',
            userFields: {
              FIELD_7097: '1',
            },
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('User fields:', result.item.userFields)
      }
    } catch (error) {
      // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
      console.error(error)
    }
    ```

- JS (UMD)

    ```html
    <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
    <script>
      async function documentUserfieldUpdate() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v3.call.make({
            method: 'catalog.document.userfield.update',
            params: {
              id: 325,
              fields: {
                docType: 'A',
                userFields: {
                  FIELD_7097: '1',
                },
              },
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('User fields:', result.item.userFields)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', documentUserfieldUpdate)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.catalog.document.userfield.update(
            bitrix_id=325,
            fields={
                "docType": "A",
                "userFields": {
                    "FIELD_7097": "1",
                },
            },
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

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via cURL or fetch.

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'catalog.document.userfield.update',
                [
                    'id' => 325,
                    'fields' => [
                        'docType' => 'A',
                        'userFields' => [
                            'FIELD_7097' => '1',
                        ],
                    ],
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        print_r($result);
    } catch (Throwable $e) {
        echo $e->getMessage();
    }
    ```

- BX24.js

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via cURL or fetch.

    ```js
    BX24.callMethod(
        'catalog.document.userfield.update',
        {
            id: 325,
            fields: {
                docType: 'A',
                userFields: {
                    FIELD_7097: '1',
                },
            },
        },
        result => {
            if (result.error())
                console.error(result.error());
            else
                console.dir(result.data());
        }
    );
    ```

- PHP CRest

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via cURL or fetch.

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'catalog.document.userfield.update',
        [
            'id' => 325,
            'fields' => [
                'docType' => 'A',
                'userFields' => [
                    'FIELD_7097' => '1',
                ],
            ],
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "catalog.document.userfield.update", b24.Params{
    	"id":     325,
    	"fields": b24.Params{
    		"docType":    "A",
    		"userFields": b24.Params{
    			"FIELD_7097": "1",
    		},
    	},
    })
    if err != nil {
    	return fmt.Errorf("catalog.document.userfield.update: %w", err)
    }

    // The method wraps the response in an object with the "item" key.
    raw, ok := b24.Unwrap(res.Result, "item")
    if !ok {
    	return fmt.Errorf("no item key in the response")
    }

    var item struct {
    	ID         int                        `json:"id"`
    	DocType    string                     `json:"docType"`
    	UserFields map[string]json.RawMessage `json:"userFields"`
    }
    if err := json.Unmarshal(raw, &item); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(item.ID, len(item.UserFields))
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "item": {
            "id": 325,
            "docType": "A",
            "userFields": {
                "FIELD_7097": "1"
            }
        }
    },
    "time": {
        "start": 1792665382,
        "finish": 1792665382.27269,
        "duration": 0.2726900577545166,
        "processing": 0,
        "date_start": "2026-10-22T13:36:22+02:00",
        "date_finish": "2026-10-22T13:36:22+02:00",
        "operating_reset_at": 1792665960,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Object with the response data ||
|| **result.item**
[`object`](../../data-types.md) | Custom field values of the document after the update. [Field description](#item) ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

#### Item Object {#item}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../data-types.md) | Document identifier ||
|| **docType**
[`string`](../../data-types.md) | Document type ||
|| **userFields**
[`object`](../../data-types.md) | Values of all custom fields of the document. The keys are `FIELD_{id}`, not `UF_*` codes as in the [catalog.document.userfield.list](./catalog-userfield-document-list-rest-v3.md) response ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": {
        "code": "BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION",
        "message": "Error validating request object.",
        "validation": [
            {
                "message": "The docType field does not match the document"
            }
        ]
    }
}
```

{% include notitle [Error handling](../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Object Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_DTOVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `docType` | Required field `docType` is empty | Pass `docType` in `fields` ||
|#

#### Request Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | At least one of the required fields must be not empty: id, filter | Pass the document `id`. This message comes together with a second one: "Only one of the following values must be provided: id, filter" ||
|| `fields` | Required field `fields` is missing | Pass the `fields` object ||
|| `-` | The docType field does not match the document | Pass the type of the document `id` in `docType` ||
|| `userFields` | This request requires data type `array` in field `userFields` | Pass `userFields` as an object ||
|#

#### Filter Format Error

Error Code: `BITRIX_REST_V3_EXCEPTION_INVALIDFILTEREXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `filter` | Invalid filter `filter` | The method does not support `filter`. Pass the document `id` ||
|#

#### Object Not Found Error

Error Code: `BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `id` | Entity with ID `#ID#` was not found | There is no document with this identifier. Check the identifier using the [catalog.document.list](../document/catalog-document-list-rest-v3.md) method ||
|#

#### Unknown Field Error

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Unknown field `#FIELD#` found in entity `UserfieldDocumentDto` | Pass only `docType` and `userFields` in `fields` ||
|#

#### Access Error

Error Code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Access denied | Check the user's permissions for documents of this type and for the Inventory Management section ||
|#

{% include [System errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-userfield-document-list-rest-v3.md)
- [{#T}](./catalog-userfield-document-field-list.md)
- [{#T}](./index.md)
