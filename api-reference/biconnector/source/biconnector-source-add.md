# Create Source biconnector.source.add

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`biconnector`](../../scopes/permissions.md)
>
> Who can execute the method: A user who has both the "Access to BI Builder" and "Access to Analytics Hub" permissions

The `biconnector.source.add` method creates a new data source linked to a connector.

{% note warning "" %}

The method works only in the context of an [application](../../../settings/app-installation/index.md) and only with the connectors that the application created itself. When called via a webhook, the method returns the `ACCESS_DENIED` error

{% endnote %}

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields***
[`object`](../../data-types.md) | An object containing data to create a new source. The object format:

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
|| **title***
[`string`](../../data-types.md) | Source name ||
|| **description**
[`string`](../../data-types.md) | Source description ||
|| **active**
[`boolean`](../../data-types.md) | Source activity.
The method ignores the passed value: the created source is always active ||
|| **connectorId***
[`integer`](../../data-types.md) | Connector identifier, can be obtained using the methods [biconnector.connector.list](../connector/biconnector-connector-list.md) or [biconnector.connector.add](../connector/biconnector-connector-add.md) ||
|| **settings***
[`object`](../../data-types.md) | Values of the authorization parameters [(detailed description)](#settings) ||
|#

When a source is created, Bitrix24 calls the connection check endpoint of the connector. If the external system does not respond, the method returns the `SOURCE_CREATE_CONNECTION_ERROR` error and the source is not created.

#### Parameter settings {#settings}

Pass `settings` as an object where the key is the `code` of a parameter declared by the connector and the value is what has to be substituted during the connection. The parameter codes can be retrieved with the [biconnector.connector.list](../connector/biconnector-connector-list.md) or [biconnector.connector.get](../connector/biconnector-connector-get.md) method. Keys that are absent from the connector description are dropped without an error.

If the connector declared parameters with the `login` and `password` codes, the field looks like this:

```json
{
    "settings": {
        "login": "admin",
        "password": "qwerty"
    }
}
```

In the response of the [biconnector.source.get](./biconnector-source-get.md) and [biconnector.source.list](./biconnector-source-list.md) methods, the same field arrives as an array of objects with the `id`, `code`, `name`, `type`, and `value` fields. Both forms are covered in the [Settings Field](./index.md#settings) section.

The [biconnector.source.fields](./biconnector-source-fields.md) method declares `settings` as the `array` type, but the `biconnector.source.add` method accepts it as an object on input.

{% note warning "" %}

The [biconnector.source.get](./biconnector-source-get.md) and [biconnector.source.list](./biconnector-source-list.md) methods return the values of the authorization parameters in plain text, including passwords and tokens

{% endnote %}

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"title":"CRM Source","description":"CRM data source","connectorId":123,"settings":{"login":"admin","password":"qwerty"}},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/biconnector.source.add
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

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type SourceAddResult = {
      id: number
    }

    try {
      const response = await $b24.actions.v2.call.make<SourceAddResult | BiconnectorError>({
        method: 'biconnector.source.add',
        params: {
          fields: {
            title: 'CRM Source',
            description: 'CRM data source',
            connectorId: 123,
            settings: {
              login: 'admin',
              password: 'qwerty',
            },
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
        if ('error' in result) {
          console.error(result.error.error, result.error.error_description)
        } else {
          console.info('Created source id:', result.id)
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
      async function addSource() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'biconnector.source.add',
            params: {
              fields: {
                title: 'CRM Source',
                description: 'CRM data source',
                connectorId: 123,
                settings: {
                  login: 'admin',
                  password: 'qwerty',
                },
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

          console.info('Created source id:', result.id)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', addSource)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.biconnector.source.add(
            fields={
                "title": "CRM Source",
                "description": "CRM data source",
                "connectorId": 123,
                "settings": {
                    "login": "admin",
                    "password": "qwerty",
                },
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
                'biconnector.source.add',
                [
                    'fields' => [
                        "title"       => "CRM Source",
                        "description" => "CRM data source",
                        "connectorId" => 123,
                        "settings"    => [
                            "login"    => "admin",
                            "password" => "qwerty"
                        ]
                    ]
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        $data = $result;

        // Methods of this section put errors inside result and answer with HTTP 200
        if (isset($data['error'])) {
            echo 'BIconnector error: ' . $data['error']['error'] . ': ' . $data['error']['error_description'];
        } else {
            echo 'Success: ' . print_r($data, true);
        }
    } catch (Throwable $e) {
        echo $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'biconnector.source.add',
        {
            fields: {
                "title": "CRM Source",
                "description": "CRM data source",
                "connectorId": 123,
                "settings": {
                    "login": "admin",
                    "password": "qwerty"
                }
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
        'biconnector.source.add',
        [
            'fields' => [
                'title' => 'CRM Source',
                'description' => 'CRM data source',
                'connectorId' => 123,
                'settings' => [
                    'login' => 'admin',
                    'password' => 'qwerty'
                ]
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
    res, err := client.Core().Call(ctx, "biconnector.source.add", b24.Params{
    	"fields": b24.Params{
    		"title":       "CRM Source",
    		"description": "CRM data source",
    		"connectorId": 123,
    		"settings": b24.Params{
    			"login":    "admin",
    			"password": "qwerty",
    		},
    	},
    })
    if err != nil {
    	return fmt.Errorf("biconnector.source.add: %w", err)
    }

    // Methods of this section put errors inside result and answer with HTTP 200.
    var apiErr struct {
    	Error *struct {
    		Error       string `json:"error"`
    		Description string `json:"error_description"`
    	} `json:"error"`
    }
    if err := json.Unmarshal(res.Result, &apiErr); err == nil && apiErr.Error != nil {
    	return fmt.Errorf("biconnector.source.add: %s: %s", apiErr.Error.Error, apiErr.Error.Description)
    }

    var item struct {
    	ID b24.ID `json:"id"`
    }
    if err := json.Unmarshal(res.Result, &item); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(item.ID)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
      "id": 7
    },
    "time": {
        "start": 1725013197.635808,
        "finish": 1725013198.580873,
        "duration": 0.9450650215148926,
        "processing": 0.6822988986968994,
        "date_start": "2024-08-30T12:19:57+02:00",
        "date_finish": "2024-08-30T12:19:58+02:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Root element of the response [(detailed description)](#result) ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

#### The result object {#result}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../data-types.md) | Identifier of the created source. Use it in the [biconnector.source.get](./biconnector-source-get.md), [biconnector.source.update](./biconnector-source-update.md), and [biconnector.source.delete](./biconnector-source-delete.md) methods ||
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

{% include notitle [Error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `ACCESS_DENIED` | Access denied. | One of the two permissions is missing, or the method was called via a webhook or outside the application context ||
|| `VALIDATION_FIELDS_NOT_PROVIDED` | Fields not provided. | Fields were not passed in the request ||
|| `VALIDATION_UNKNOWN_PARAMETERS` | Unknown parameters: #LIST_OF_PARAMS# | Unknown parameters detected: list ||
|| `VALIDATION_REQUIRED_FIELD_MISSING` | Field "#TITLE#" is required. | Required field #TITLE# was not provided ||
|| `VALIDATION_READ_ONLY_FIELD` | Field "#TITLE#" is read only. | Field #TITLE# is read-only and cannot be modified ||
|| `VALIDATION_INVALID_FIELD_TYPE` | Field "#TITLE#" must be of type #TYPE#. | Field #TITLE# must be of type #TYPE# ||
|| `CONNECTOR_NOT_FOUND` | Connector was not found. | The connector does not exist or belongs to another application ||
|| `SOURCE_CREATE_CONNECTION_ERROR` | Cannot create connection. | The external system did not respond to the request to the connection check endpoint — the source was not created ||

|#

{% include [System errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./biconnector-source-update.md)
- [{#T}](./biconnector-source-get.md)
- [{#T}](./biconnector-source-list.md)
- [{#T}](./biconnector-source-delete.md)
- [{#T}](./biconnector-source-fields.md)
