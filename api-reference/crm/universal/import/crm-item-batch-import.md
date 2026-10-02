# Importing a Batch of CRM Records: crm.item.batchImport

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with permission to import CRM items

The `crm.item.batchImport` method imports up to 20 items of the same CRM type.

Pass the fields of each item according to the same rules as in [crm.item.import](crm-item-import.md). Import specifics are described in the [method overview](./index.md).

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type`          | **Description** ||
|| **entityTypeId***
[`integer`](../../../data-types.md) | Identifier of the [system](../../data-types.md#object_type) or [custom CRM type](../user-defined-object-types/index.md) into which the items should be imported.

Numerical values for system types, such as lead — `1`, deal — `2`, contact — `3`, company — `4`, and invoice — `31`, are provided in the [CRM object types reference](../../data-types.md#object_type). You can obtain a SPA identifier using [crm.type.list](../user-defined-object-types/crm-type-list.md). ||
|| **data***
[`array`](../../../data-types.md) | Array of objects with the fields of the items being imported [(detailed description)](#data) ||
|| **useOriginalUfNames**
[`boolean`](../../../data-types.md) | Parameter to control the format of custom field names in the request.
Possible values:

- `Y` — original names of custom fields, e.g., `UF_CRM_2_1639669411830`
- `N` — custom field names in camelCase, e.g., `ufCrm2_1639669411830`

Default is `N`. ||
|#

### Parameter data {#data}

Each element of the `data` array is an object containing the fields of one CRM item:

```js
[
    {
        field_1: value_1,
        field_2: value_2
    },
    {
        field_1: value_1,
        field_2: value_2
    }
]
```

Field names, types, and formats are described in the [`fields`](crm-item-import.md#fields) parameter of `crm.item.import`.

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

1. How to Import Deals

    {% list tabs %}

    - cURL (Webhook)

        ```bash
        curl -X POST \
        -H "Content-Type: application/json" \
        -H "Accept: application/json" \
        -d '{"entityTypeId":2,"data":[{"title":"First imported deal","isRecurring":"N","opportunity":999.99,"currencyId":"EUR"},{"title":"Second imported deal","isRecurring":"N","opportunity":1499.99,"currencyId":"EUR"}]}' \
        https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.item.batchImport
        ```

    - cURL (OAuth)

        ```bash
        curl -X POST \
        -H "Content-Type: application/json" \
        -H "Accept: application/json" \
        -d '{"entityTypeId":2,"data":[{"title":"First imported deal","isRecurring":"N","opportunity":999.99,"currencyId":"EUR"},{"title":"Second imported deal","isRecurring":"N","opportunity":1499.99,"currencyId":"EUR"}],"auth":"**put_access_token_here**"}' \
        https://**put_your_bitrix24_address**/rest/crm.item.batchImport
        ```

    - JS (TS)

        ```ts
        import { Text } from '@bitrix24/b24jssdk'
        import type { B24Frame } from '@bitrix24/b24jssdk'
        declare const $b24: B24Frame

        const response = await $b24.actions.v2.call.make({
          method: 'crm.item.batchImport',
          params: {
            entityTypeId: 2,
            data: [
              { title: 'First imported deal', isRecurring: 'N', opportunity: 999.99, currencyId: 'EUR' },
              { title: 'Second imported deal', isRecurring: 'N', opportunity: 1499.99, currencyId: 'EUR' },
            ],
          },
          requestId: Text.getUuidRfc4122()
        })
        console.info(response.getData()?.result)
        ```

    - JS (UMD)

        ```html
        <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
        <script>
          async function batchImportDeals() {
            const $b24 = await B24Js.initializeB24Frame()
            const response = await $b24.actions.v2.call.make({
              method: 'crm.item.batchImport',
              params: {
                entityTypeId: 2,
                data: [
                  { title: 'First imported deal', isRecurring: 'N', opportunity: 999.99, currencyId: 'EUR' },
                  { title: 'Second imported deal', isRecurring: 'N', opportunity: 1499.99, currencyId: 'EUR' },
                ],
              },
              requestId: B24Js.Text.getUuidRfc4122()
            })
            console.info(response.getData()?.result)
          }
          document.addEventListener('DOMContentLoaded', batchImportDeals)
        </script>
        ```

    - Python

        ```python

        from b24pysdk.errors import BitrixAPIError, BitrixSDKException

        try:
            bitrix_response = client.crm.item.batch_import(
                entity_type_id=2,
                data=[
                    {
                        "title": "First imported deal",
                        "isRecurring": "N",
                        "opportunity": 999.99,
                        "currencyId": "EUR",
                    },
                    {
                        "title": "Second imported deal",
                        "isRecurring": "N",
                        "opportunity": 1499.99,
                        "currencyId": "EUR",
                    },
                ],
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
            $response = $b24Service->core->call(
                'crm.item.batchImport',
                [
                    'entityTypeId' => 2,
                    'data' => [
                        ['title' => 'First imported deal', 'isRecurring' => 'N', 'opportunity' => 999.99, 'currencyId' => 'EUR'],
                        ['title' => 'Second imported deal', 'isRecurring' => 'N', 'opportunity' => 1499.99, 'currencyId' => 'EUR'],
                    ],
                ]
            );
            echo 'Success: ' . print_r($response->getResponseData()->getResult()->data(), true);
        } catch (Throwable $e) {
            echo 'Error: ' . $e->getMessage();
        }
        ```

    - BX24.js

        ```js
        BX24.callMethod(
            'crm.item.batchImport',
            {
                entityTypeId: 2,
                data: [
                    {
                        title: 'First imported deal',
                        isRecurring: 'N',
                        opportunity: 999.99,
                        currencyId: 'EUR',
                    },
                    {
                        title: 'Second imported deal',
                        isRecurring: 'N',
                        opportunity: 1499.99,
                        currencyId: 'EUR',
                    },
                ],
            },
            result => result.error() ? console.error(result.error()) : console.info(result.data())
        );
        ```
    - PHP CRest

        ```php
        require_once('crest.php');

        $result = CRest::call(
            'crm.item.batchImport',
            [
                'entityTypeId' => 2,
                'data' => [
                    ['title' => 'First imported deal', 'isRecurring' => 'N', 'opportunity' => 999.99, 'currencyId' => 'EUR'],
                    ['title' => 'Second imported deal', 'isRecurring' => 'N', 'opportunity' => 1499.99, 'currencyId' => 'EUR'],
                ],
            ]
        );

        print_r($result);
        ```
    - Go

        ```go
        // client and ctx are already created — see the Go SDK section
        res, err := client.Core().Call(ctx, "crm.item.batchImport", b24.Params{
            "entityTypeId": 2,
            "data": []b24.Params{
                {"title": "First imported deal", "isRecurring": "N", "opportunity": 999.99, "currencyId": "EUR"},
                {"title": "Second imported deal", "isRecurring": "N", "opportunity": 1499.99, "currencyId": "EUR"},
            },
        })
        if err != nil {
            return fmt.Errorf("crm.item.batchImport: %w", err)
        }

        raw, ok := b24.Unwrap(res.Result, "items")
        if !ok {
            return fmt.Errorf("items key is missing from the response")
        }
        fmt.Printf("%s\n", raw)
        ```

    {% endlist %}



2. How to Create an SPA Element with a Set of Custom Fields

    {% cut "Custom fields involved in the example" %}

    {% include [Set of Custom Fields](../../_include/user-fields-for-examples-cut.md) %}

    {% endcut %}

    {% list tabs %}

    - cURL (Webhook)

        ```bash
        curl -X POST \
        -H "Content-Type: application/json" \
        -H "Accept: application/json" \
        -d '{
            "entityTypeId": 1302,
            "data": [{
                "ufCrm44_1721812760630": "String value for a custom String field",
                "ufCrm44_1721812814433": 81,
                "ufCrm44_1721812853419": "2024-08-21",
                "ufCrm44_1721812885588": [
                    "example.com",
                    "second-example.com"
                ],
                "ufCrm44_1721812898903": [
                    "green_pixel.png",
                    "iVBORw0KGgoAAAANSUhEUgAAAIAAAAAMCAYAAACqTLVoAAAALklEQVR42u3SAQEAAAQDsEsuOj3YMqwy6fBWCSCAAAIgAAIgAAIgAAIgAAJw3QLOrRH1U/gU4gAAAABJRU5ErkJggg=="
                ],
                "ufCrm44_1721812915476": "300|EUR",
                "ufCrm44_1721812935209": "Y",
                "ufCrm44_1721812948498": 9999.9
            },{
                "ufCrm44_1721812760630": "String value for a custom String field",
                "ufCrm44_1721812814433": 45,
                "ufCrm44_1721812853419": "2024-08-21",
                "ufCrm44_1721812885588": [
                    "example.com",
                    "second-example.com"
                ],
                "ufCrm44_1721812898903": [
                    "green_pixel2.png",
                    "iVBORw0KGgoAAAANSUhEUgAAAIAAAAAMCAYAAACqTLVoAAAALklEQVR42u3SAQEAAAQDsEsuOj3YMqwy6fBWCSCAAAIgAAIgAAIgAAIgAAJw3QLOrRH1U/gU4gAAAABJRU5ErkJggg=="
                ],
                "ufCrm44_1721812915476": "600|EUR",
                "ufCrm44_1721812935209": "N",
                "ufCrm44_1721812948498": 9999.9
            }]
        }' \
        https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.item.batchImport
        ```

    - cURL (OAuth)

        ```bash
        curl -X POST \
        -H "Content-Type: application/json" \
        -H "Accept: application/json" \
        -d '{
            "entityTypeId": 1302,
            "data": [{
                "ufCrm44_1721812760630": "String value for a custom String field",
                "ufCrm44_1721812814433": 81,
                "ufCrm44_1721812853419": "2024-08-21",
                "ufCrm44_1721812885588": [
                    "example.com",
                    "second-example.com"
                ],
                "ufCrm44_1721812898903": [
                    "green_pixel.png",
                    "iVBORw0KGgoAAAANSUhEUgAAAIAAAAAMCAYAAACqTLVoAAAALklEQVR42u3SAQEAAAQDsEsuOj3YMqwy6fBWCSCAAAIgAAIgAAIgAAIgAAJw3QLOrRH1U/gU4gAAAABJRU5ErkJggg=="
                ],
                "ufCrm44_1721812915476": "300|EUR",
                "ufCrm44_1721812935209": "Y",
                "ufCrm44_1721812948498": 9999.9
            },{
                "ufCrm44_1721812760630": "String value for a custom String field",
                "ufCrm44_1721812814433": 45,
                "ufCrm44_1721812853419": "2024-08-21",
                "ufCrm44_1721812885588": [
                    "example.com",
                    "second-example.com"
                ],
                "ufCrm44_1721812898903": [
                    "green_pixel2.png",
                    "iVBORw0KGgoAAAANSUhEUgAAAIAAAAAMCAYAAACqTLVoAAAALklEQVR42u3SAQEAAAQDsEsuOj3YMqwy6fBWCSCAAAIgAAIgAAIgAAIgAAJw3QLOrRH1U/gU4gAAAABJRU5ErkJggg=="
                ],
                "ufCrm44_1721812915476": "600|EUR",
                "ufCrm44_1721812935209": "N",
                "ufCrm44_1721812948498": 9999.9
            }],
            "auth": "**put_access_token_here**"
        }' \
        https://**put_your_bitrix24_address**/rest/crm.item.batchImport
        ```

    - JS (TS)

        ```ts
        // This snippet is an ES module: top-level await requires type="module" or a bundler.
        // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
        import { Text } from '@bitrix24/b24jssdk'
        import type { B24Frame } from '@bitrix24/b24jssdk'

        declare const $b24: B24Frame

        // Shape of the payload returned in result (match the "response handling" section of the page)
        type BatchImportResult = {
          items: Array<
            | { item: { id: number } }
            | { error: string; error_description: string }
          >
        }

        const greenPixelInBase64 = "iVBORw0KGgoAAAANSUhEUgAAAIAAAAAMCAYAAACqTLVoAAAALklEQVR42u3SAQEAAAQDsEsuOj3YMqwy6fBWCSCAAAIgAAIgAAIgAAIgAAJw3QLOrRH1U/gU4gAAAABJRU5ErkJggg=="

        try {
          const response = await $b24.actions.v2.call.make<BatchImportResult>({
            method: 'crm.item.batchImport',
            params: {
              entityTypeId: 1302,
              data: [
                {
                  ufCrm44_1721812760630: "String value for a custom String field",
                  ufCrm44_1721812814433: 81,
                  ufCrm44_1721812853419: "2024-08-21",
                  ufCrm44_1721812885588: [
                    "example.com",
                    "second-example.com",
                  ],
                  ufCrm44_1721812898903: [
                    "green_pixel.png",
                    greenPixelInBase64,
                  ],
                  ufCrm44_1721812915476: "300|EUR",
                  ufCrm44_1721812935209: "Y",
                  ufCrm44_1721812948498: 9999.9,
                },
                {
                  ufCrm44_1721812760630: "String value for a custom String field",
                  ufCrm44_1721812814433: 45,
                  ufCrm44_1721812853419: "2024-08-21",
                  ufCrm44_1721812885588: [
                    "example.com",
                    "second-example.com",
                  ],
                  ufCrm44_1721812898903: [
                    "green_pixel2.png",
                    greenPixelInBase64,
                  ],
                  ufCrm44_1721812915476: "600|EUR",
                  ufCrm44_1721812935209: "N",
                  ufCrm44_1721812948498: 9999.9,
                },
              ],
            },
            requestId: Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
          } else {
            const result = response.getData()!.result
            console.info('Imported items:', result.items)
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
          async function batchImportItems() {
            try {
              // Initialize the SDK inside a Bitrix24 frame
              const $b24 = await B24Js.initializeB24Frame()

              const greenPixelInBase64 = "iVBORw0KGgoAAAANSUhEUgAAAIAAAAAMCAYAAACqTLVoAAAALklEQVR42u3SAQEAAAQDsEsuOj3YMqwy6fBWCSCAAAIgAAIgAAIgAAIgAAJw3QLOrRH1U/gU4gAAAABJRU5ErkJggg=="

              const response = await $b24.actions.v2.call.make({
                method: 'crm.item.batchImport',
                params: {
                  entityTypeId: 1302,
                  data: [
                    {
                      ufCrm44_1721812760630: "String value for a custom String field",
                      ufCrm44_1721812814433: 81,
                      ufCrm44_1721812853419: "2024-08-21",
                      ufCrm44_1721812885588: [
                        "example.com",
                        "second-example.com",
                      ],
                      ufCrm44_1721812898903: [
                        "green_pixel.png",
                        greenPixelInBase64,
                      ],
                      ufCrm44_1721812915476: "300|EUR",
                      ufCrm44_1721812935209: "Y",
                      ufCrm44_1721812948498: 9999.9,
                    },
                    {
                      ufCrm44_1721812760630: "String value for a custom String field",
                      ufCrm44_1721812814433: 45,
                      ufCrm44_1721812853419: "2024-08-21",
                      ufCrm44_1721812885588: [
                        "example.com",
                        "second-example.com",
                      ],
                      ufCrm44_1721812898903: [
                        "green_pixel2.png",
                        greenPixelInBase64,
                      ],
                      ufCrm44_1721812915476: "600|EUR",
                      ufCrm44_1721812935209: "N",
                      ufCrm44_1721812948498: 9999.9,
                    },
                  ],
                },
                requestId: B24Js.Text.getUuidRfc4122()
              })

              // The payload is available only on a successful response
              if (!response.isSuccess) {
                console.error(response.getErrorMessages().join('; '))
                return
              }

              const result = response.getData().result
              console.info('Imported items:', result.items)
            } catch (error) {
              // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
              console.error(error)
            }
          }

          document.addEventListener('DOMContentLoaded', batchImportItems)
        </script>
        ```

    - Python

        ```python

        from b24pysdk.errors import BitrixAPIError, BitrixSDKException

        try:
            bitrix_response = client.crm.item.batch_import(
                entity_type_id=1302,
                data=[
                    {
                    "ufCrm44_1721812760630": "String for a custom field of the String type",
                    "ufCrm44_1721812814433": 81,
                    "ufCrm44_1721812853419": "2024-08-21",
                    "ufCrm44_1721812885588": [
                        "example.com",
                        "second-example.com",
                    ],
                    "ufCrm44_1721812898903": [
                        "green_pixel.png",
                        "iVBORw0KGgoAAAANSUhEUgAAAIAAAAAMCAYAAACqTLVoAAAALklEQVR42u3SAQEAAAQDsEsuOj3YMqwy6fBWCSCAAAIgAAIgAAIgAAIgAAJw3QLOrRH1U/gU4gAAAABJRU5ErkJggg==",
                    ],
                    "ufCrm44_1721812915476": "300|EUR",
                    "ufCrm44_1721812935209": "Y",
                    "ufCrm44_1721812948498": 9999.9,
                },
                    {
                    "ufCrm44_1721812760630": "String for a custom field of the String type",
                    "ufCrm44_1721812814433": 45,
                    "ufCrm44_1721812853419": "2024-08-21",
                    "ufCrm44_1721812885588": [
                        "example.com",
                        "second-example.com",
                    ],
                    "ufCrm44_1721812898903": [
                        "green_pixel2.png",
                        "iVBORw0KGgoAAAANSUhEUgAAAIAAAAAMCAYAAACqTLVoAAAALklEQVR42u3SAQEAAAQDsEsuOj3YMqwy6fBWCSCAAAIgAAIgAAIgAAIgAAJw3QLOrRH1U/gU4gAAAABJRU5ErkJggg==",
                    ],
                    "ufCrm44_1721812915476": "600|EUR",
                    "ufCrm44_1721812935209": "N",
                    "ufCrm44_1721812948498": 9999.9,
                },
                ],
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
        require_once('crest.php');

        $result = CRest::call(
            'crm.item.batchImport',
            [
                'entityTypeId' => 1302,
                'data' => [
                    [
                        'ufCrm44_1721812760630' => "String value for a custom String field",
                        'ufCrm44_1721812814433' => 81,
                        'ufCrm44_1721812853419' => '2024-08-21',
                        'ufCrm44_1721812885588' => [
                            "example.com",
                            "second-example.com",
                        ],
                        'ufCrm44_1721812898903' => [
                            "green_pixel.png",
                            "iVBORw0KGgoAAAANSUhEUgAAAIAAAAAMCAYAAACqTLVoAAAALklEQVR42u3SAQEAAAQDsEsuOj3YMqwy6fBWCSCAAAIgAAIgAAIgAAIgAAJw3QLOrRH1U/gU4gAAAABJRU5ErkJggg==",
                        ],
                        'ufCrm44_1721812915476' => "300|EUR",
                        'ufCrm44_1721812935209' => "Y",
                        'ufCrm44_1721812948498' => 9999.9,
                    ],
                    [
                        'ufCrm44_1721812760630' => "String value for a custom String field",
                        'ufCrm44_1721812814433' => 45,
                        'ufCrm44_1721812853419' => '2024-08-21',
                        'ufCrm44_1721812885588' => [
                            "example.com",
                            "second-example.com",
                        ],
                        'ufCrm44_1721812898903' => [
                            "green_pixel2.png",
                            "iVBORw0KGgoAAAANSUhEUgAAAIAAAAAMCAYAAACqTLVoAAAALklEQVR42u3SAQEAAAQDsEsuOj3YMqwy6fBWCSCAAAIgAAIgAAIgAAIgAAJw3QLOrRH1U/gU4gAAAABJRU5ErkJggg==",
                        ],
                        'ufCrm44_1721812915476' => "600|EUR",
                        'ufCrm44_1721812935209' => "N",
                        'ufCrm44_1721812948498' => 9999.9,
                    ],
                ],
            ]
        );

        echo '<PRE>';
        print_r($result);
        echo '</PRE>';
        ```

    - BX24.js

        ```js
        BX24.callMethod(
            'crm.item.batchImport',
            {
                entityTypeId: 1302,
                data: [
                    {
                        ufCrm44_1721812760630: 'String value for a custom String field',
                        ufCrm44_1721812814433: 81,
                        ufCrm44_1721812853419: '2024-08-21',
                        ufCrm44_1721812885588: ['example.com', 'second-example.com'],
                        ufCrm44_1721812898903: ['green_pixel.png', 'iVBORw0KGgoAAAANSUhEUgAAAIAAAAAMCAYAAACqTLVoAAAALklEQVR42u3SAQEAAAQDsEsuOj3YMqwy6fBWCSCAAAIgAAIgAAIgAAIgAAJw3QLOrRH1U/gU4gAAAABJRU5ErkJggg=='],
                        ufCrm44_1721812915476: '300|EUR',
                        ufCrm44_1721812935209: 'Y',
                        ufCrm44_1721812948498: 9999.9,
                    },
                    {
                        ufCrm44_1721812760630: 'String value for a custom String field',
                        ufCrm44_1721812814433: 45,
                        ufCrm44_1721812853419: '2024-08-21',
                        ufCrm44_1721812885588: ['example.com', 'second-example.com'],
                        ufCrm44_1721812898903: ['green_pixel2.png', 'iVBORw0KGgoAAAANSUhEUgAAAIAAAAAMCAYAAACqTLVoAAAALklEQVR42u3SAQEAAAQDsEsuOj3YMqwy6fBWCSCAAAIgAAIgAAIgAAIgAAJw3QLOrRH1U/gU4gAAAABJRU5ErkJggg=='],
                        ufCrm44_1721812915476: '600|EUR',
                        ufCrm44_1721812935209: 'N',
                        ufCrm44_1721812948498: 9999.9,
                    },
                ],
            },
            result => result.error() ? console.error(result.error()) : console.info(result.data())
        );
        ```

    - PHP CRest

        ```php
        require_once('crest.php');

        $result = CRest::call(
            'crm.item.batchImport',
            [
                'entityTypeId' => 1302,
                'data' => [
                    [
                        'ufCrm44_1721812760630' => "String value for a custom String field",
                        'ufCrm44_1721812814433' => 81,
                        'ufCrm44_1721812853419' => '2024-08-21',
                        'ufCrm44_1721812885588' => [
                            "example.com",
                            "second-example.com",
                        ],
                        'ufCrm44_1721812898903' => [
                            "green_pixel.png",
                            "iVBORw0KGgoAAAANSUhEUgAAAIAAAAAMCAYAAACqTLVoAAAALklEQVR42u3SAQEAAAQDsEsuOj3YMqwy6fBWCSCAAAIgAAIgAAIgAAIgAAJw3QLOrRH1U/gU4gAAAABJRU5ErkJggg==",
                        ],
                        'ufCrm44_1721812915476' => "300|EUR",
                        'ufCrm44_1721812935209' => "Y",
                        'ufCrm44_1721812948498' => 9999.9,
                    ],
                    [
                        'ufCrm44_1721812760630' => "String value for a custom String field",
                        'ufCrm44_1721812814433' => 45,
                        'ufCrm44_1721812853419' => '2024-08-21',
                        'ufCrm44_1721812885588' => [
                            "example.com",
                            "second-example.com",
                        ],
                        'ufCrm44_1721812898903' => [
                            "green_pixel2.png",
                            "iVBORw0KGgoAAAANSUhEUgAAAIAAAAAMCAYAAACqTLVoAAAALklEQVR42u3SAQEAAAQDsEsuOj3YMqwy6fBWCSCAAAIgAAIgAAIgAAIgAAJw3QLOrRH1U/gU4gAAAABJRU5ErkJggg==",
                        ],
                        'ufCrm44_1721812915476' => "600|EUR",
                        'ufCrm44_1721812935209' => "N",
                        'ufCrm44_1721812948498' => 9999.9,
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
        res, err := client.Core().Call(ctx, "crm.item.batchImport", b24.Params{
            "entityTypeId": 1302,
            "data": []b24.Params{
                {
                    "ufCrm44_1721812760630": "String value for a custom String field",
                    "ufCrm44_1721812814433": 81,
                    "ufCrm44_1721812853419": "2024-08-21",
                    "ufCrm44_1721812885588": []string{"example.com", "second-example.com"},
                    "ufCrm44_1721812898903": []string{"green_pixel.png", "iVBORw0KGgoAAAANSUhEUgAAAIAAAAAMCAYAAACqTLVoAAAALklEQVR42u3SAQEAAAQDsEsuOj3YMqwy6fBWCSCAAAIgAAIgAAIgAAIgAAJw3QLOrRH1U/gU4gAAAABJRU5ErkJggg=="},
                    "ufCrm44_1721812915476": "300|EUR",
                    "ufCrm44_1721812935209": "Y",
                    "ufCrm44_1721812948498": 9999.9,
                },
                {
                    "ufCrm44_1721812760630": "String value for a custom String field",
                    "ufCrm44_1721812814433": 45,
                    "ufCrm44_1721812853419": "2024-08-21",
                    "ufCrm44_1721812885588": []string{"example.com", "second-example.com"},
                    "ufCrm44_1721812898903": []string{"green_pixel2.png", "iVBORw0KGgoAAAANSUhEUgAAAIAAAAAMCAYAAACqTLVoAAAALklEQVR42u3SAQEAAAQDsEsuOj3YMqwy6fBWCSCAAAIgAAIgAAIgAAIgAAJw3QLOrRH1U/gU4gAAAABJRU5ErkJggg=="},
                    "ufCrm44_1721812915476": "600|EUR",
                    "ufCrm44_1721812935209": "N",
                    "ufCrm44_1721812948498": 9999.9,
                },
            },
        })
        if err != nil {
            return fmt.Errorf("crm.item.batchImport: %w", err)
        }

        // The method wraps the response in an object with the "items" key.
        raw, ok := b24.Unwrap(res.Result, "items")
        if !ok {
        	return fmt.Errorf("no items key in the response")
        }

        fmt.Printf("%s\n", raw)
        ```

    {% endlist %}


## Response Handling

The method returns an `items` array. Each array element contains an `item` object with the identifier of the created item or the `error` and `error_description` fields with import error details.

HTTP status: **200**

```json
{
    "result": {
        "items": [
            {
                "item": {
                    "id": 15
                }
            },
            {
                "error": "CRM_FIELD_ERROR_REQUIRED",
                "error_description": "The \"Name\" field is required"
            }
        ]
    },
    "time": {
        "start": 1723414961.913589,
        "finish": 1723414964.652124,
        "duration": 2.738534927368164,
        "processing": 2.376383066177368,
        "date_start": "2024-08-11T22:22:41+00:00",
        "date_finish": "2024-08-11T22:22:44+00:00",
        "operating": 2.3762991428375244
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../data-types.md) | Root element of the response. Contains the import results [(detailed description)](#result) ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

#### Result Object {#result}

#|
|| **Name**
`type` | **Description** ||
|| **items**
[`array`](../../../data-types.md) | Item import results [(detailed description)](#items) ||
|#

#### Items Array Element {#items}

Each array element contains either an `item` object if the import succeeds or the `error` and `error_description` fields if the import fails.

#|
|| **Name**
`type` | **Description** ||
|| **item**
[`object`](../../../data-types.md) | Successful import result [(detailed description)](#item) ||
|| **error**
[`string`](../../../data-types.md) | Item import error code ||
|| **error_description**
[`string`](../../../data-types.md) | Item import error description ||
|#

#### Item Object {#item}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../../data-types.md) | Identifier of the created item ||
|#

## Error Handling

HTTP status: **400**, **401**, **403**

```json
{
    "error": "NOT_FOUND",
    "error_description": "Smart process not found"
}
```

{% include notitle [Error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `NOT_FOUND` | SPA not found | An unknown `entityTypeId` was passed ||
|| `400` | `ACCESS_DENIED` | Access denied | The user does not have permission to import items of type `entityTypeId` ||
|| `400` | `CRM_FIELD_ERROR_VALUE_NOT_VALID` | Invalid value for field `field` | An invalid value was passed for the `field` field.

For system fields, such as `createdTime`, the error also occurs if the request is made by a non-administrator ||
|| `400` | `100` | Expected iterable value for multiple field, but got `type` instead | A value of type `type` was passed to a multiple field, but an iterable value was expected. The error can also occur due to invalid JSON or request headers ||
|| `400` | `CREATE_DYNAMIC_ITEM_RESTRICTED` | You cannot create a new item due to your plan restrictions | Plan restrictions do not allow creating SPA items ||
|| `400` | `MAX_IMPORT_BATCH_SIZE_EXCEEDED` | You cannot import more than 20 items | The `data` array contains more than 20 items ||
|| `401` | `INVALID_CREDENTIALS` | Invalid authorization data for the request | Invalid user identifier or webhook code in the request URL ||
|| `403` | `allowed_only_intranet_user` | This action is allowed only for intranet users | The user is not an intranet user ||
|#

{% include [System errors](./../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./crm-item-import.md)
