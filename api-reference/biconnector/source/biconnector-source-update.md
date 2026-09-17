# Update Source biconnector.source.update

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`biconnector`](../../scopes/permissions.md)
>
> Who can execute the method: A user who has both the "Access to BI Builder" and "Access to Analytics Hub" permissions

The method `biconnector.source.update` updates an existing source.

{% note warning "" %}

The method works only in the context of an [application](../../../settings/app-installation/index.md) and changes only the sources that the application created itself. When called via a webhook, the method returns the `ACCESS_DENIED` error

{% endnote %}

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../data-types.md) | Identifier of the source, can be obtained using the methods [biconnector.source.list](./biconnector-source-list.md) and [biconnector.source.add](./biconnector-source-add.md) ||
|| **fields***
[`object`](../../data-types.md) | Object containing the updated data.
The object format:

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
[`string`](../../data-types.md) | New name of the source ||
|| **description**
[`string`](../../data-types.md) | New description of the source ||
|| **active**
[`boolean`](../../data-types.md) | Source activity.
The method does not read this field: a source cannot be switched off via REST ||
|| **settings**
[`object`](../../data-types.md) | Values of the authorization parameters [(detailed description)](#settings) ||
|#

The method changes only the fields that were passed: whatever you did not pass stays unchanged in the source. This is also true for `settings` — the settings are merged by key, so pass only the authorization parameters that have to be changed.

The `connectorId` field is set once when the source is created and cannot be changed with the `biconnector.source.update` method.

On every update, Bitrix24 calls the connection check endpoint of the connector, even if the request contained no `settings`. If the external system does not respond, the method returns the `SOURCE_UPDATE_CONNECTION_ERROR` error.

#### Parameter settings {#settings}

Pass `settings` as an object where the key is the `code` of a parameter declared by the connector and the value is what has to be substituted during the connection. The parameter codes can be retrieved with the [biconnector.connector.list](../connector/biconnector-connector-list.md) or [biconnector.connector.get](../connector/biconnector-connector-get.md) method. Keys that are absent from the connector description are dropped without an error.

If the connector declared parameters with the `login` and `password` codes, the field looks like this:

```json
{
    "settings": {
        "login": "new_admin",
        "password": "new_password"
    }
}
```

In the response of the [biconnector.source.get](./biconnector-source-get.md) and [biconnector.source.list](./biconnector-source-list.md) methods, the same field arrives as an array of objects with the `id`, `code`, `name`, `type`, and `value` fields. Both forms are covered in the [Settings Field](./index.md#settings) section.

The [biconnector.source.fields](./biconnector-source-fields.md) method declares `settings` as the `array` type, but the `biconnector.source.update` method accepts it as an object on input.

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
    -d '{
        "id": 4,
        "fields": {
            "title": "New source name",
            "description": "Updated source description",
            "settings": {
                "login": "new_admin",
                "password": "new_password"
            }
        },
        "auth": "**put_access_token_here**"
    }' \
    https://**put_your_bitrix24_address**/rest/biconnector.source.update
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
        method: 'biconnector.source.update',
        params: {
          id: 4,
          fields: {
            title: 'New source name',
            description: 'Updated source description',
            settings: {
              login: 'new_admin',
              password: 'new_password',
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
        if (typeof result === 'object' && result !== null && 'error' in result) {
          console.error(result.error.error, result.error.error_description)
        } else {
          console.info('Source updated:', result)
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
      async function updateSource() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'biconnector.source.update',
            params: {
              id: 4,
              fields: {
                title: 'New source name',
                description: 'Updated source description',
                settings: {
                  login: 'new_admin',
                  password: 'new_password',
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

          console.info('Source updated:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', updateSource)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.biconnector.source.update(
            bitrix_id=4,
            fields={
                "title": "New source name",
                "description": "Updated source description",
                "settings": {
                    "login": "new_admin",
                    "password": "new_password",
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
                'biconnector.source.update',
                [
                    'id' => 4,
                    'fields' => [
                        "title"       => "New source name",
                        "description" => "Updated source description",
                        "settings"    => [
                            "login"    => "new_admin",
                            "password" => "new_password"
                        ]
                    ]
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        if ($result->error()) {
            error_log($result->error());
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
        echo 'Error updating source: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'biconnector.source.update',
        {
            id: 4,
            fields: {
                "title": "New source name",
                "description": "Updated source description",
                "settings": {
                    "login": "new_admin",
                    "password": "new_password"
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
        'biconnector.source.update',
        [
            'id' => 4,
            'fields' => [
                'title' => 'New source name',
                'description' => 'Updated source description',
                'settings' => [
                    'login' => 'new_admin',
                    'password' => 'new_password'
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
    res, err := client.Core().Call(ctx, "biconnector.source.update", b24.Params{
    	"id": 4,
    	"fields": b24.Params{
    		"title":       "New source name",
    		"description": "Updated source description",
    		"settings": b24.Params{
    			"login":    "new_admin",
    			"password": "new_password",
    		},
    	},
    })
    if err != nil {
    	return fmt.Errorf("biconnector.source.update: %w", err)
    }

    // Methods of this section put errors inside result and answer with HTTP 200.
    var apiErr struct {
    	Error *struct {
    		Error       string `json:"error"`
    		Description string `json:"error_description"`
    	} `json:"error"`
    }
    if err := json.Unmarshal(res.Result, &apiErr); err == nil && apiErr.Error != nil {
    	return fmt.Errorf("biconnector.source.update: %s: %s", apiErr.Error.Error, apiErr.Error.Description)
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
[`boolean`](../../data-types.md) | Update result. On a successful update, `true` arrives and the method does not return the source data — retrieve it with the [biconnector.source.get](./biconnector-source-get.md) method. On an error, an object with the `error` field arrives in `result` instead of `true`, see the "Error Handling" section ||
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
|| `VALIDATION_FIELDS_NOT_PROVIDED` | Fields not provided. | Fields not passed in the request ||
|| `VALIDATION_UNKNOWN_PARAMETERS` | Unknown parameters: #LIST_OF_PARAMS# | Unknown parameters detected: list ||
|| `VALIDATION_READ_ONLY_FIELD` | Field "#TITLE#" is read only. | Field #TITLE# is read-only and cannot be modified ||
|| `VALIDATION_IMMUTABLE_FIELD` | Field "#TITLE#" is immutable. | Field #TITLE# is immutable ||
|| `VALIDATION_INVALID_FIELD_TYPE` | Field "#TITLE#" must be of type #TYPE#. | Field #TITLE# must be of type #TYPE# ||
|| `SOURCE_NOT_FOUND` | Source was not found. | The source does not exist or belongs to another application ||
|| `SOURCE_UPDATE_CONNECTION_ERROR` | Cannot update connection. | The external system did not respond to the request to the connection check endpoint — the source was not updated ||
|| Empty value | All the fields are required. | The required `title` parameter was not passed. The REST-level validation does not catch it, so the error comes from the module and has no string code of its own ||

|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./biconnector-source-add.md)
- [{#T}](./biconnector-source-get.md)
- [{#T}](./biconnector-source-list.md)
- [{#T}](./biconnector-source-delete.md)
- [{#T}](./biconnector-source-fields.md)
