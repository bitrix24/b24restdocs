# Get Access Key List biconnector.key.list

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`biconnector`](../../scopes/permissions.md)
>
> Who can execute the method: a user with the permission to manage BI connector keys, for example an administrator

The method `biconnector.key.list` retrieves the list of BI connector access keys that the application created.

{% note warning "" %}

The method works only in the context of an [application](../../../settings/app-installation/index.md). When called via a webhook, the method returns the `insufficient_auth_type` error

{% endnote %}

## Method Parameters

#|
|| **Name**
`type` | **Description** ||
|| **select**
[`array`](../../data-types.md) | Key fields to return. Allowed values: `ID`, `ACTIVE`, `DATE_CREATE`, `TIMESTAMP_X`, `CREATED_BY`, `CONNECTION`, `ACCESS_KEY`. The fields are described in the [methods overview](./index.md#fields).

The `ACCESS_KEY` key value is returned only if the key was created by the user on whose behalf the method is called. For other keys, the response has no `ACCESS_KEY` field.

Unknown fields are ignored. If the parameter is not passed or contains no allowed fields other than `ACCESS_KEY`, all fields are returned ||
|| **filter**
[`object`](../../data-types.md) | Filter in the format `{"field": "value"}`. You can filter by the `ID`, `ACTIVE`, `DATE_CREATE`, `TIMESTAMP_X`, `CREATED_BY`, `CONNECTION`, and `ACCESS_KEY` fields. You can put an operator before the field name, for example `>=DATE_CREATE` or `!ACTIVE`.

If the filter has a condition on `ACCESS_KEY`, the method searches only among the keys created by the current user.

Conditions on other fields are ignored ||
|| **order**
[`object`](../../data-types.md) | Sorting in the format `{"field": "direction"}`, for example `{"ID": "DESC"}`. The direction is `ASC` or `DESC` ||
|| **limit**
[`integer`](../../data-types.md) | Number of keys to return.

Default — `50` ||
|| **offset**
[`integer`](../../data-types.md) | Number of keys to skip from the start of the selection.

Default — `0` ||
|#

The method does not return the total number of keys. To get the next page, increase `offset` by `limit`. If fewer keys than `limit` are returned, this is the last page.

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (OAuth)

    ```bash
    curl -X POST \
         -H "Content-Type: application/json" \
         -H "Accept: application/json" \
         -d '{
             "filter": {
                 "ACTIVE": "Y"
             },
             "order": {
                 "ID": "DESC"
             },
             "limit": 5,
             "auth": "**put_access_token_here**"
             }' \
         https://**put_your_bitrix24_address**/rest/biconnector.key.list
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type BiconnectorKey = {
      ID: string
      ACTIVE: 'Y' | 'N'
      DATE_CREATE: string
      TIMESTAMP_X: string
      CREATED_BY: string
      CONNECTION: string
      // Present only for keys created by the current user
      ACCESS_KEY?: string
    }

    try {
      const response = await $b24.actions.v2.call.make<BiconnectorKey[]>({
        method: 'biconnector.key.list',
        params: {
          filter: {
            ACTIVE: 'Y',
          },
          order: {
            ID: 'DESC',
          },
          limit: 5,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        for (const key of response.getData()!.result) {
          console.info(key.ID, key.ACTIVE, key.ACCESS_KEY ?? 'value is hidden')
        }
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
      async function listKeys() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'biconnector.key.list',
            params: {
              filter: {
                ACTIVE: 'Y',
              },
              order: {
                ID: 'DESC',
              },
              limit: 5,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          for (const key of response.getData().result) {
            console.info(key.ID, key.ACTIVE, key.ACCESS_KEY ?? 'value is hidden')
          }
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', listKeys)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        # b24pysdk has no ready-made wrapper for biconnector.key.*, so the method
        # is called directly through bitrix_token.call_method()
        response = bitrix_token.call_method(
            api_method="biconnector.key.list",
            params={
                "filter": {
                    "ACTIVE": "Y",
                },
                "order": {
                    "ID": "DESC",
                },
                "limit": 5,
            },
        )
        for key in response["result"]:
            print(key["ID"], key["ACTIVE"], key.get("ACCESS_KEY", "value is hidden"))
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
        $response = $b24Service
            ->core
            ->call(
                'biconnector.key.list',
                [
                    'filter' => [
                        'ACTIVE' => 'Y',
                    ],
                    'order' => [
                        'ID' => 'DESC',
                    ],
                    'limit' => 5,
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        foreach ($result as $key) {
            echo $key['ID'] . ': ' . ($key['ACCESS_KEY'] ?? 'value is hidden') . PHP_EOL;
        }
    } catch (Throwable $e) {
        echo $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'biconnector.key.list',
        {
            filter: {
                ACTIVE: 'Y',
            },
            order: {
                ID: 'DESC',
            },
            limit: 5,
        },
        (result) => {
            if (result.error()) {
                console.error(result.error());
                return;
            }

            console.info(result.data());
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'biconnector.key.list',
        [
            'filter' => [
                'ACTIVE' => 'Y',
            ],
            'order' => [
                'ID' => 'DESC',
            ],
            'limit' => 5,
        ]
    );

    if (isset($result['error'])) {
        echo 'Error: ' . $result['error'] . ': ' . $result['error_description'];
    } else {
        echo '<PRE>';
        print_r($result);
        echo '</PRE>';
    }
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "biconnector.key.list", b24.Params{
    	"filter": b24.Params{
    		"ACTIVE": "Y",
    	},
    	"order": b24.Params{
    		"ID": "DESC",
    	},
    	"limit": 5,
    })
    if err != nil {
    	return fmt.Errorf("biconnector.key.list: %w", err)
    }

    var keys []struct {
    	ID         string `json:"ID"`
    	Active     string `json:"ACTIVE"`
    	DateCreate string `json:"DATE_CREATE"`
    	CreatedBy  string `json:"CREATED_BY"`
    	Connection string `json:"CONNECTION"`
    	AccessKey  string `json:"ACCESS_KEY,omitempty"` // empty if the key was created by another user
    }
    if err := json.Unmarshal(res.Result, &keys); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    for _, k := range keys {
    	fmt.Println(k.ID, k.Active, k.AccessKey)
    }
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": [
        {
            "ID": "15",
            "ACTIVE": "Y",
            "DATE_CREATE": "2026-11-24T14:02:26+01:00",
            "TIMESTAMP_X": "2026-11-24T14:02:39+01:00",
            "CREATED_BY": "1",
            "CONNECTION": "bi",
            "ACCESS_KEY": "a7Kq2Lm9Xc4Rt8Vb1Nz6Hy3Pw5Js0Df"
        }
    ],
    "time": {
        "start": 1795525362,
        "finish": 1795525362.411178,
        "duration": 0.4111781120300293,
        "processing": 0,
        "date_start": "2026-11-24T14:02:42+01:00",
        "date_finish": "2026-11-24T14:02:42+01:00",
        "operating_reset_at": 1795525920,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`array`](../../data-types.md) | Array of keys with the fields from the `select` parameter. The fields are described in the [methods overview](./index.md#fields). If there are no keys, an empty array is returned ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "100",
    "error_description": "Invalid order \"WRONG\""
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `100` | Invalid order "#ORDER#" | The sort direction in `order` is neither `ASC` nor `DESC` ||
|| `400` | `insufficient_auth_type` | The request requires higher privileges than provided. | The method was called via a webhook, not from an application ||
|| `400` | `ACCESS_DENIED` | Access denied | The user does not have the permission to manage BI connector keys ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./biconnector-key-add.md)
- [{#T}](./biconnector-key-update.md)
- [{#T}](./biconnector-key-delete.md)
