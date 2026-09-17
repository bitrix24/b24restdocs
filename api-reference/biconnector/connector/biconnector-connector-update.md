# Update Connector biconnector.connector.update

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`biconnector`](../../scopes/permissions.md)
>
> Who can execute the method: A user who has both the "Access to BI Builder" and "Access to Analytics Hub" permissions

The method `biconnector.connector.update` updates an existing connector.

{% note warning "" %}

The method works only in the context of an [application](../../../settings/app-installation/index.md) and changes only the connectors that the application created itself. When called via a webhook, the method returns the `ACCESS_DENIED` error

{% endnote %}

The update is partial: the fields you did not pass keep their previous values. The `settings` array is an exception: it is replaced entirely, so pass all connection parameters in it, not only the changed ones.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../data-types.md) | Connector identifier, can be obtained using the methods [biconnector.connector.list](./biconnector-connector-list.md) and [biconnector.connector.add](./biconnector-connector-add.md) ||
|| **fields***
[`object`](../../data-types.md) | An object containing the data to update. The object format:

```
{
    "field_1": "value_1",
    "field_2": "value_2",
    ...,
    "field_n": "value_n"
}
```

- `field_n` — field name
- `value_n` — field value

[Detailed description below](#fields) ||
|#

### Parameter fields {#fields}

#|
|| **Name**
`type` | **Description** ||
|| **title**
[`string`](../../data-types.md) | New connector name, maximum length is 512 characters ||
|| **logo**
[`string`](../../data-types.md) | New connector logo. Can be passed as a link to an image or a base64 formatted string, for example `data:image/svg+xml;base64,PHN2ZyB3...` ||
|| **description**
[`string`](../../data-types.md) | New connector description ||
|| **urlCheck**
[`string`](../../data-types.md) | New endpoint for checking the connector availability, maximum length is 2048 characters, [(detailed description)](./index.md#urlCheck) ||
|| **urlTableList**
[`string`](../../data-types.md) | New endpoint for retrieving the list of tables, maximum length is 2048 characters, [(detailed description)](./index.md#urlTableList) ||
|| **urlTableDescription**
[`string`](../../data-types.md) | New endpoint for retrieving the description of a specific table, maximum length is 2048 characters, [(detailed description)](./index.md#urlTableDescription) ||
|| **urlData**
[`string`](../../data-types.md) | New endpoint for retrieving data from the selected table, maximum length is 2048 characters, [(detailed description)](./index.md#urlData) ||
|| **settings**
[`array`](../../data-types.md) | New array of connection parameters. It replaces the previous one entirely [(detailed description)](#settings) ||
|| **supportMapping**
[`boolean`](../../data-types.md) | Support for mapping the table fields to the fields of the external system. If the field is not passed, the connector keeps its previous value.

The value is checked with a strict comparison against the boolean type, so it does not pass in a request of the `application/x-www-form-urlencoded` type — send it with the `Content-Type: application/json` header ||
|| **sourceCode**
[`string`](../../data-types.md) | String code of the external system, maximum length is 64 characters ||
|| **sort**
[`integer`](../../data-types.md) | New sorting parameter for the connector ||
|#

### Parameter settings {#settings}

Each element of the `settings` array is an object with three required fields.

#|
|| **Name**
`type` | **Description** ||
|| **code***
[`string`](../../data-types.md) | Parameter code. The parameter goes to the external system under this name inside the `connection` object. Maximum length is 512 characters ||
|| **name***
[`string`](../../data-types.md) | Parameter name that the user sees in the Analytics hub section. Maximum length is 512 characters ||
|| **type***
[`string`](../../data-types.md) | Parameter type that determines the input field in the interface. Allowed values: `STRING`, `INT`. The value is case-sensitive: a lowercase `string` causes the `VALIDATION_SETTINGS_INVALID_TYPE` error ||
|#

## Code Examples

{% include [Footnote on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (OAuth)

    ```bash
    curl -X POST \
         -H "Content-Type: application/json" \
         -H "Accept: application/json" \
         -d '{
             "id": 4,
             "fields": {
                 "title": "UPDATED REST CONNECTOR",
                 "logo": "data:image/svg+xml;base64,PHN2ZyB3aWR0aD0iMjIiIGhlaWdodD0iMjIiIHZpZXdCb3g9IjAgMCAyMiAyMiIgZmlsbD0ibm9uZSIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIj4KCTxjaXJjbGUgY3g9IjExIiBjeT0iMTEiIHI9IjEwIiBmaWxsPSIjMkZDN0Y3IiAvPgoJPHRleHQgeD0iMTEiIHk9IjEzIiBmb250LWZhbWlseT0iQXJpYWwsIHNhbnMtc2VyaWYiIGZvbnQtc2l6ZT0iNiIgZmlsbD0iI0ZGRkZGRiIgdGV4dC1hbmNob3I9Im1pZGRsZSIgZm9udC13ZWlnaHQ9ImJvbGQiPlJFU1Q8L3RleHQ+Cjwvc3ZnPgo=",
                 "description": "Updated description",
                 "urlCheck": "http://example.com/api/new_check",
                 "urlTableList": "http://example.com/api/new_table_list",
                 "urlTableDescription": "http://example.com/api/new_table_description",
                 "urlData": "http://example.com/api/new_data",
                 "settings": [
                    {
                        "name": "Employee Identifier",
                        "type": "STRING",
                        "code": "id"
                    },
                    {
                        "name": "Password",
                        "type": "STRING",
                        "code": "password"
                    }
                 ],
                 "sort": 200
             },
             "auth": "**put_access_token_here**"
             }' \
         https://**put_your_bitrix24_address**/rest/biconnector.connector.update
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Methods of this section put errors inside result and answer with HTTP 200
    type BiconnectorError = {
      error: {
        error: string
        error_description: string
      }
    }

    try {
      const response = await $b24.actions.v2.call.make<boolean | BiconnectorError>({
        method: 'biconnector.connector.update',
        params: {
          id: 4,
          fields: {
            title: 'UPDATED REST CONNECTOR',
            logo: 'data:image/svg+xml;base64,PHN2ZyB3aWR0aD0iMjIiIGhlaWdodD0iMjIiIHZpZXdCb3g9IjAgMCAyMiAyMiIgZmlsbD0ibm9uZSIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIj4KCTxjaXJjbGUgY3g9IjExIiBjeT0iMTEiIHI9IjEwIiBmaWxsPSIjMkZDN0Y3IiAvPgoJPHRleHQgeD0iMTEiIHk9IjEzIiBmb250LWZhbWlseT0iQXJpYWwsIHNhbnMtc2VyaWYiIGZvbnQtc2l6ZT0iNiIgZmlsbD0iI0ZGRkZGRiIgdGV4dC1hbmNob3I9Im1pZGRsZSIgZm9udC13ZWlnaHQ9ImJvbGQiPlJFU1Q8L3RleHQ+Cjwvc3ZnPgo=',
            description: 'Updated description',
            urlCheck: 'http://example.com/api/new_check',
            urlTableList: 'http://example.com/api/new_table_list',
            urlTableDescription: 'http://example.com/api/new_table_description',
            urlData: 'http://example.com/api/new_data',
            settings: [
              {
                name: 'Employee ID',
                type: 'STRING',
                code: 'id',
              },
              {
                name: 'Password',
                type: 'STRING',
                code: 'password',
              },
            ],
            sort: 200,
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result

        // The SDK sees HTTP 200 as success, so check the error inside result yourself
        if (typeof result === 'object' && result !== null && 'error' in result) {
          console.error(result.error.error, result.error.error_description)
        } else {
          console.info('Connector updated:', result)
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
      async function updateConnector() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'biconnector.connector.update',
            params: {
              id: 4,
              fields: {
                title: 'UPDATED REST CONNECTOR',
                logo: 'data:image/svg+xml;base64,PHN2ZyB3aWR0aD0iMjIiIGhlaWdodD0iMjIiIHZpZXdCb3g9IjAgMCAyMiAyMiIgZmlsbD0ibm9uZSIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIj4KCTxjaXJjbGUgY3g9IjExIiBjeT0iMTEiIHI9IjEwIiBmaWxsPSIjMkZDN0Y3IiAvPgoJPHRleHQgeD0iMTEiIHk9IjEzIiBmb250LWZhbWlseT0iQXJpYWwsIHNhbnMtc2VyaWYiIGZvbnQtc2l6ZT0iNiIgZmlsbD0iI0ZGRkZGRiIgdGV4dC1hbmNob3I9Im1pZGRsZSIgZm9udC13ZWlnaHQ9ImJvbGQiPlJFU1Q8L3RleHQ+Cjwvc3ZnPgo=',
                description: 'Updated description',
                urlCheck: 'http://example.com/api/new_check',
                urlTableList: 'http://example.com/api/new_table_list',
                urlTableDescription: 'http://example.com/api/new_table_description',
                urlData: 'http://example.com/api/new_data',
                settings: [
                  {
                    name: 'Employee ID',
                    type: 'STRING',
                    code: 'id',
                  },
                  {
                    name: 'Password',
                    type: 'STRING',
                    code: 'password',
                  },
                ],
                sort: 200,
              },
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result

          // The SDK sees HTTP 200 as success, so check the error inside result yourself
          if (result && result.error) {
            console.error(result.error.error, result.error.error_description)
            return
          }

          console.info('Connector updated:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', updateConnector)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.biconnector.connector.update(
            bitrix_id=4,
            fields={
                "title": "UPDATED REST CONNECTOR",
                "logo": "data:image/svg+xml;base64,PHN2ZyB3aWR0aD0iMjIiIGhlaWdodD0iMjIiIHZpZXdCb3g9IjAgMCAyMiAyMiIgZmlsbD0ibm9uZSIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIj4KCTxjaXJjbGUgY3g9IjExIiBjeT0iMTEiIHI9IjEwIiBmaWxsPSIjMkZDN0Y3IiAvPgoJPHRleHQgeD0iMTEiIHk9IjEzIiBmb250LWZhbWlseT0iQXJpYWwsIHNhbnMtc2VyaWYiIGZvbnQtc2l6ZT0iNiIgZmlsbD0iI0ZGRkZGRiIgdGV4dC1hbmNob3I9Im1pZGRsZSIgZm9udC13ZWlnaHQ9ImJvbGQiPlJFU1Q8L3RleHQ+Cjwvc3ZnPgo=",
                "description": "Updated description",
                "urlCheck": "http://example.com/api/new_check",
                "urlTableList": "http://example.com/api/new_table_list",
                "urlTableDescription": "http://example.com/api/new_table_description",
                "urlData": "http://example.com/api/new_data",
                "settings": [
                    {
                        "name": "Employee ID",
                        "type": "STRING",
                        "code": "id",
                    },
                    {
                        "name": "Password",
                        "type": "STRING",
                        "code": "password",
                    },
                ],
                "sort": 200,
            },
        ).response
        result = bitrix_response.result

        # Methods of this section put errors inside result and answer with HTTP 200
        if isinstance(result, dict) and "error" in result:
            print(
                "BIconnector error",
                f"error: {result['error']['error']}",
                f"error_description: {result['error']['error_description']}",
                sep="\n",
            )
        else:
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
        $response = $b24Service
            ->core
            ->call(
                'biconnector.connector.update',
                [
                    'id' => 4,
                    'fields' => [
                        "title"               => "UPDATED REST CONNECTOR",
                        "logo"                => "data:image/svg+xml;base64,PHN2ZyB3aWR0aD0iMjIiIGhlaWdodD0iMjIiIHZpZXdCb3g9IjAgMCAyMiAyMiIgZmlsbD0ibm9uZSIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIj4KCTxjaXJjbGUgY3g9IjExIiBjeT0iMTEiIHI9IjEwIiBmaWxsPSIjMkZDN0Y3IiAvPgoJPHRleHQgeD0iMTEiIHk9IjEzIiBmb250LWZhbWlseT0iQXJpYWwsIHNhbnMtc2VyaWYiIGZvbnQtc2l6ZT0iNiIgZmlsbD0iI0ZGRkZGRiIgdGV4dC1hbmNob3I9Im1pZGRsZSIgZm9udC13ZWlnaHQ9ImJvbGQiPlJFU1Q8L3RleHQ+Cjwvc3ZnPgo=",
                        "description"         => "Updated description",
                        "urlCheck"            => "http://example.com/api/new_check",
                        "urlTableList"        => "http://example.com/api/new_table_list",
                        "urlTableDescription" => "http://example.com/api/new_table_description",
                        "urlData"             => "http://example.com/api/new_data",
                        "settings"            => [
                            [
                                "name" => "Employee Identifier",
                                "type" => "STRING",
                                "code" => "id"
                            ],
                            [
                                "name" => "Password",
                                "type" => "STRING",
                                "code" => "password"
                            ]
                        ],
                        "sort"                => 200
                    ]
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        if ($result->error()) {
            echo 'Error: ' . $result->error();
        } else {
            $data = $result->data();

            // Methods of this section put errors inside result and answer with HTTP 200
            if (isset($data['error'])) {
                echo 'BIconnector error: ' . $data['error']['error'] . ': ' . $data['error']['error_description'];
            } else {
                echo 'Success: ' . print_r($data, true);
            }
        }

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error updating connector: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'biconnector.connector.update',
        {
            id: 4,
            fields: {
                "title": "UPDATED REST CONNECTOR",
                "logo": "data:image/svg+xml;base64,PHN2ZyB3aWR0aD0iMjIiIGhlaWdodD0iMjIiIHZpZXdCb3g9IjAgMCAyMiAyMiIgZmlsbD0ibm9uZSIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIj4KCTxjaXJjbGUgY3g9IjExIiBjeT0iMTEiIHI9IjEwIiBmaWxsPSIjMkZDN0Y3IiAvPgoJPHRleHQgeD0iMTEiIHk9IjEzIiBmb250LWZhbWlseT0iQXJpYWwsIHNhbnMtc2VyaWYiIGZvbnQtc2l6ZT0iNiIgZmlsbD0iI0ZGRkZGRiIgdGV4dC1hbmNob3I9Im1pZGRsZSIgZm9udC13ZWlnaHQ9ImJvbGQiPlJFU1Q8L3RleHQ+Cjwvc3ZnPgo=",
                "description": "Updated description",
                "urlCheck": "http://example.com/api/new_check",
                "urlTableList": "http://example.com/api/new_table_list",
                "urlTableDescription": "http://example.com/api/new_table_description",
                "urlData": "http://example.com/api/new_data",
                "settings": [
                    {
                        "name": "Employee Identifier",
                        "type": "STRING",
                        "code": "id"
                    },
                    {
                        "name": "Password",
                        "type": "STRING",
                        "code": "password"
                    }
                ],
                "sort": 200
            }
        },
        (result) => {
            if (result.error()) {
                console.error(result.error());
                return;
            }

            const data = result.data();

            // Methods of this section put errors inside result and answer with HTTP 200
            if (data && data.error) {
                console.error(data.error.error, data.error.error_description);
                return;
            }

            console.info(data);
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'biconnector.connector.update',
        [
            'id' => 4,
            'fields' => [
                'title' => 'UPDATED REST CONNECTOR',
                'logo' => 'data:image/svg+xml;base64,PHN2ZyB3aWR0aD0iMjIiIGhlaWdodD0iMjIiIHZpZXdCb3g9IjAgMCAyMiAyMiIgZmlsbD0ibm9uZSIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIj4KCTxjaXJjbGUgY3g9IjExIiBjeT0iMTEiIHI9IjEwIiBmaWxsPSIjMkZDN0Y3IiAvPgoJPHRleHQgeD0iMTEiIHk9IjEzIiBmb250LWZhbWlseT0iQXJpYWwsIHNhbnMtc2VyaWYiIGZvbnQtc2l6ZT0iNiIgZmlsbD0iI0ZGRkZGRiIgdGV4dC1hbmNob3I9Im1pZGRsZSIgZm9udC13ZWlnaHQ9ImJvbGQiPlJFU1Q8L3RleHQ+Cjwvc3ZnPgo=',
                'description' => 'Updated description',
                'urlCheck' => 'http://example.com/api/new_check',
                'urlTableList' => 'http://example.com/api/new_table_list',
                'urlTableDescription' => 'http://example.com/api/new_table_description',
                'urlData' => 'http://example.com/api/new_data',
                'settings' => [
                    [
                        'name' => 'Employee Identifier',
                        'type' => 'STRING',
                        'code' => 'id'
                    ],
                    [
                        'name' => 'Password',
                        'type' => 'STRING',
                        'code' => 'password'
                    ]
                ],
                'sort' => 200
            ]
        ]
    );

    // Methods of this section put errors inside result and answer with HTTP 200
    if (isset($result['result']['error'])) {
        echo 'BIconnector error: ' . $result['result']['error']['error']
            . ': ' . $result['result']['error']['error_description'];
    } else {
        echo '<PRE>';
        print_r($result);
        echo '</PRE>';
    }
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "biconnector.connector.update", b24.Params{
    	"id": 4,
    	"fields": b24.Params{
    		"title":               "UPDATED REST CONNECTOR",
    		"logo":                "data:image/svg+xml;base64,PHN2ZyB3aWR0aD0iMjIiIGhlaWdodD0iMjIiIHZpZXdCb3g9IjAgMCAyMiAyMiIgZmlsbD0ibm9uZSIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIj4KCTxjaXJjbGUgY3g9IjExIiBjeT0iMTEiIHI9IjEwIiBmaWxsPSIjMkZDN0Y3IiAvPgoJPHRleHQgeD0iMTEiIHk9IjEzIiBmb250LWZhbWlseT0iQXJpYWwsIHNhbnMtc2VyaWYiIGZvbnQtc2l6ZT0iNiIgZmlsbD0iI0ZGRkZGRiIgdGV4dC1hbmNob3I9Im1pZGRsZSIgZm9udC13ZWlnaHQ9ImJvbGQiPlJFU1Q8L3RleHQ+Cjwvc3ZnPgo=",
    		"description":         "Updated description",
    		"urlCheck":            "http://example.com/api/new_check",
    		"urlTableList":        "http://example.com/api/new_table_list",
    		"urlTableDescription": "http://example.com/api/new_table_description",
    		"urlData":             "http://example.com/api/new_data",
    		"settings": []b24.Params{
    			{
    				"name": "Employee Identifier",
    				"type": "STRING",
    				"code": "id",
    			},
    			{
    				"name": "Password",
    				"type": "STRING",
    				"code": "password",
    			},
    		},
    		"sort": 200,
    	},
    })
    if err != nil {
    	return fmt.Errorf("biconnector.connector.update: %w", err)
    }

    // Methods of this section put errors inside result and answer with HTTP 200.
    var apiErr struct {
    	Error *struct {
    		Error       string `json:"error"`
    		Description string `json:"error_description"`
    	} `json:"error"`
    }
    if err := json.Unmarshal(res.Result, &apiErr); err == nil && apiErr.Error != nil {
    	return fmt.Errorf("biconnector.connector.update: %s: %s", apiErr.Error.Error, apiErr.Error.Description)
    }

    var ok bool
    if err := json.Unmarshal(res.Result, &ok); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println("done:", ok)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": true,
    "time": {
        "start": 1725365418.056843,
        "finish": 1725365419.671506,
        "duration": 1.6146628856658936,
        "processing": 1.3475170135498047,
        "date_start": "2024-09-03T14:10:18+02:00",
        "date_finish": "2024-09-03T14:10:19+02:00"
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../data-types.md) | Root element of the response. On a successful update it contains `true` — the response carries no object with the connector data. To see the new field values, call [biconnector.connector.get](./biconnector-connector-get.md) ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **200**

```json
{
    "result": {
        "error": {
            "error": "VALIDATION_FIELDS_NOT_PROVIDED",
            "error_description": "Fields not provided."
        }
    }
}
```

{% note warning "" %}

The method returns an error [inside the `result` field](../index.md#errors) and with HTTP status 200. Check `result.error`: the SDK wrappers parse only the top level of the response and treat such an error as a success

{% endnote %}

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `ACCESS_DENIED` | Access denied. | One of the two permissions is missing, or the method was called via a webhook or outside the application context ||
|| `VALIDATION_ID_NOT_PROVIDED` | ID is missing. | Identifier is not specified ||
|| `VALIDATION_INVALID_ID_FORMAT` | ID has to be a positive integer. | Invalid ID format ||
|| `VALIDATION_FIELDS_NOT_PROVIDED` | Fields not provided. | Fields were not passed in the request ||
|| `VALIDATION_UNKNOWN_PARAMETERS` | Unknown parameters: #LIST_OF_PARAMS# | Unknown parameters detected: list ||
|| `VALIDATION_READ_ONLY_FIELD` | Field "#TITLE#" is read only. | Field #TITLE# is read-only and cannot be modified ||
|| `VALIDATION_INVALID_FIELD_TYPE` | Field "#TITLE#" must be of type #TYPE#. | Field #TITLE# must be of type #TYPE# ||
|| `CONNECTOR_NOT_FOUND` | Connector was not found. | The connector does not exist or belongs to another application ||

|| `VALIDATION_SETTINGS_MISSING_REQUIRED_FIELDS` | Settings must include "type", "name" and "code" fields. | Settings must include the fields `type`, `name`, and `code` ||
|| `VALIDATION_SETTINGS_INVALID_TYPE` | Parameter "type" is not correct. | Invalid value for parameter `type` ||
|| `VALIDATION_SETTINGS_NAME_TOO_LONG` | Parameter "name" must be less than 512 characters. | The value of parameter `name` must not exceed 512 characters ||
|| `VALIDATION_SETTINGS_CODE_TOO_LONG` | Parameter "code" must be less than 512 characters. | The value of parameter `code` must not exceed 512 characters ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./biconnector-connector-add.md)
- [{#T}](./biconnector-connector-get.md)
- [{#T}](./biconnector-connector-list.md)
- [{#T}](./biconnector-connector-delete.md)
- [{#T}](./biconnector-connector-fields.md)