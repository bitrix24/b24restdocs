# Update Cash Register Handler sale.cashbox.handler.update

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`sale, cashbox`](../../scopes/permissions.md)
>
> Who can execute the method: CRM administrator (permission "Allow to change settings")

This method updates the data of the REST cash register handler.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **ID*** 
[`sale_cashbox_handler.ID`](../data-types.md#sale_cashbox_handler) | Identifier of the handler being updated ||
|| **FIELDS*** 
[`object`](../../data-types.md) | Values of the fields to be updated.

Fields available for update: `NAME`, `SORT`, `SETTINGS` (see fields [sale_cashbox_handler](../data-types.md#sale_cashbox_handler)) 
||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"ID":1,"FIELDS":{"NAME":"My REST cash register with a new name","SORT":200,"SETTINGS":{"PRINT_URL":"http://setagaya.bx/receipt_print.php","CHECK_URL":"http://setagaya.bx/receipt_check.php","CONFIG":{"AUTH":{"LABEL":"Authorization","ITEMS":{"LOGIN":{"TYPE":"STRING","REQUIRED":"Y","LABEL":"Login"},"PASSWORD":{"TYPE":"STRING","REQUIRED":"Y","LABEL":"Password"}}},"COMPANY":{"LABEL":"Company Information","ITEMS":{"INN":{"TYPE":"STRING","REQUIRED":"Y","LABEL":"Company INN"}}},"INTERACTION":{"LABEL":"Cash Register Interaction Settings","ITEMS":{"MODE":{"TYPE":"ENUM","LABEL":"Cash Register Operation Mode","OPTIONS":{"ACTIVE":"live","TEST":"test"}}}}},"SUPPORTS_FFD105":"N"}}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sale.cashbox.handler.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"ID":1,"FIELDS":{"NAME":"My REST cash register with a new name","SORT":200,"SETTINGS":{"PRINT_URL":"http://setagaya.bx/receipt_print.php","CHECK_URL":"http://setagaya.bx/receipt_check.php","CONFIG":{"AUTH":{"LABEL":"Authorization","ITEMS":{"LOGIN":{"TYPE":"STRING","REQUIRED":"Y","LABEL":"Login"},"PASSWORD":{"TYPE":"STRING","REQUIRED":"Y","LABEL":"Password"}}},"COMPANY":{"LABEL":"Company Information","ITEMS":{"INN":{"TYPE":"STRING","REQUIRED":"Y","LABEL":"Company INN"}}},"INTERACTION":{"LABEL":"Cash Register Interaction Settings","ITEMS":{"MODE":{"TYPE":"ENUM","LABEL":"Cash Register Operation Mode","OPTIONS":{"ACTIVE":"live","TEST":"test"}}}}},"SUPPORTS_FFD105":"N"}},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.cashbox.handler.update
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    try {
      const response = await $b24.actions.v2.call.make<boolean>({
        method: 'sale.cashbox.handler.update',
        params: {
          ID: 1,
          FIELDS: {
            NAME: 'My REST cashbox with a new name',
            SORT: 200,
            SETTINGS: {
              PRINT_URL: 'http://setagaya.bx/receipt_print.php',
              CHECK_URL: 'http://setagaya.bx/receipt_check.php',
              CONFIG: {
                AUTH: {
                  LABEL: 'Authorization',
                  ITEMS: {
                    LOGIN: {
                      TYPE: 'STRING',
                      REQUIRED: 'Y',
                      LABEL: 'Login',
                    },
                    PASSWORD: {
                      TYPE: 'STRING',
                      REQUIRED: 'Y',
                      LABEL: 'Password',
                    },
                  },
                },
                COMPANY: {
                  LABEL: 'Organization data',
                  ITEMS: {
                    INN: {
                      TYPE: 'STRING',
                      REQUIRED: 'Y',
                      LABEL: 'Organization INN',
                    },
                  },
                },
                INTERACTION: {
                  LABEL: 'Cashbox interaction settings',
                  ITEMS: {
                    MODE: {
                      TYPE: 'ENUM',
                      LABEL: 'Cashbox operating mode',
                      OPTIONS: {
                        ACTIVE: 'production',
                        TEST: 'test',
                      },
                    },
                  },
                },
              },
              SUPPORTS_FFD105: 'N',
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
        console.info('Handler updated:', result)
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
      async function updateCashboxHandler() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'sale.cashbox.handler.update',
            params: {
              ID: 1,
              FIELDS: {
                NAME: 'My REST cashbox with a new name',
                SORT: 200,
                SETTINGS: {
                  PRINT_URL: 'http://setagaya.bx/receipt_print.php',
                  CHECK_URL: 'http://setagaya.bx/receipt_check.php',
                  CONFIG: {
                    AUTH: {
                      LABEL: 'Authorization',
                      ITEMS: {
                        LOGIN: {
                          TYPE: 'STRING',
                          REQUIRED: 'Y',
                          LABEL: 'Login',
                        },
                        PASSWORD: {
                          TYPE: 'STRING',
                          REQUIRED: 'Y',
                          LABEL: 'Password',
                        },
                      },
                    },
                    COMPANY: {
                      LABEL: 'Organization data',
                      ITEMS: {
                        INN: {
                          TYPE: 'STRING',
                          REQUIRED: 'Y',
                          LABEL: 'Organization INN',
                        },
                      },
                    },
                    INTERACTION: {
                      LABEL: 'Cashbox interaction settings',
                      ITEMS: {
                        MODE: {
                          TYPE: 'ENUM',
                          LABEL: 'Cashbox operating mode',
                          OPTIONS: {
                            ACTIVE: 'production',
                            TEST: 'test',
                          },
                        },
                      },
                    },
                  },
                  SUPPORTS_FFD105: 'N',
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
          console.info('Handler updated:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', updateCashboxHandler)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'sale.cashbox.handler.update',
                [
                    'ID'     => 1,
                    'FIELDS' => [
                        'NAME'     => 'My REST cash register with a new name',
                        'SORT'     => 200,
                        'SETTINGS' => [
                            'PRINT_URL'      => 'http://setagaya.bx/receipt_print.php',
                            'CHECK_URL'      => 'http://setagaya.bx/receipt_check.php',
                            'CONFIG'         => [
                                'AUTH'        => [
                                    'LABEL' => 'Authorization',
                                    'ITEMS' => [
                                        'LOGIN'    => [
                                            'TYPE'     => 'STRING',
                                            'REQUIRED' => 'Y',
                                            'LABEL'    => 'Login',
                                        ],
                                        'PASSWORD' => [
                                            'TYPE'     => 'STRING',
                                            'REQUIRED' => 'Y',
                                            'LABEL'    => 'Password',
                                        ],
                                    ],
                                ],
                                'COMPANY'      => [
                                    'LABEL' => 'Company Information',
                                    'ITEMS' => [
                                        'INN' => [
                                            'TYPE'     => 'STRING',
                                            'REQUIRED' => 'Y',
                                            'LABEL'    => 'Company INN',
                                        ],
                                    ],
                                ],
                                'INTERACTION'  => [
                                    'LABEL' => 'Cash Register Interaction Settings',
                                    'ITEMS' => [
                                        'MODE' => [
                                            'TYPE'    => 'ENUM',
                                            'LABEL'   => 'Cash Register Operation Mode',
                                            'OPTIONS' => [
                                                'ACTIVE' => 'live',
                                                'TEST'   => 'test',
                                            ],
                                        ],
                                    ],
                                ],
                            ],
                            'SUPPORTS_FFD105' => 'N',
                        ],
                    ],
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
        echo 'Error updating cash register handler: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod( 
    "sale.cashbox.handler.update", 
    { 
        "ID": 1, 
        "FIELDS": 
        { 
            "NAME": "My REST cash register with a new name",
            "SORT": 200,
            "SETTINGS": {
                "PRINT_URL": "http://setagaya.bx/receipt_print.php",
                "CHECK_URL": "http://setagaya.bx/receipt_check.php",
                "CONFIG": {
                    "AUTH": {
                        "LABEL": "Authorization",
                        "ITEMS": {
                            "LOGIN": {
                                "TYPE": "STRING",
                                "REQUIRED": "Y",
                                "LABEL": "Login"
                            },
                            "PASSWORD": {
                                "TYPE": "STRING",
                                "REQUIRED": "Y",
                                "LABEL": "Password"
                            }
                        }
                    },
                    "COMPANY": {
                        "LABEL": "Company Information",
                        "ITEMS": {
                            "INN": {
                                "TYPE": "STRING",
                                "REQUIRED": "Y",
                                "LABEL": "Company INN"
                            }
                        }
                    },
                    "INTERACTION": {
                        "LABEL": "Cash Register Interaction Settings",
                        "ITEMS": {
                            "MODE": {
                                "TYPE": "ENUM",
                                "LABEL": "Cash Register Operation Mode",
                                "OPTIONS": {
                                    "ACTIVE": "live",
                                    "TEST": "test"
                                }
                            }
                        }
                    }
                },
                "SUPPORTS_FFD105": "N"
            }
        } 
    }, 
        function(result) 
    { 
        if(result.error()) 
            console.error(result.error()); 
        else 
            console.dir(result.data()); 
    } 
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'sale.cashbox.handler.update',
        [
            'ID' => 1,
            'FIELDS' =>
            [
                'NAME' => 'My REST cash register with a new name',
                'SORT' => 200,
                'SETTINGS' =>
                [
                    'PRINT_URL' => 'http://setagaya.bx/receipt_print.php',
                    'CHECK_URL' => 'http://setagaya.bx/receipt_check.php',
                    'CONFIG' =>
                    [
                        'AUTH' =>
                        [
                            'LABEL' => 'Authorization',
                            'ITEMS' =>
                            [
                                'LOGIN' =>
                                [
                                    'TYPE' => 'STRING',
                                    'REQUIRED' => 'Y',
                                    'LABEL' => 'Login'
                                ],
                                'PASSWORD' =>
                                [
                                    'TYPE' => 'STRING',
                                    'REQUIRED' => 'Y',
                                    'LABEL' => 'Password'
                                ]
                            ]
                        },
                        'COMPANY' =>
                        [
                            'LABEL' => 'Company Information',
                            'ITEMS' =>
                            [
                                'INN' =>
                                [
                                    'TYPE' => 'STRING',
                                    'REQUIRED' => 'Y',
                                    'LABEL' => 'Company INN'
                                ]
                            ]
                        },
                        'INTERACTION' =>
                        [
                            'LABEL' => 'Cash Register Interaction Settings',
                            'ITEMS' =>
                            [
                                'MODE' =>
                                [
                                    'TYPE' => 'ENUM',
                                    'LABEL' => 'Cash Register Operation Mode',
                                    'OPTIONS' =>
                                    [
                                        'ACTIVE' => 'live',
                                        'TEST' => 'test'
                                    ]
                                ]
                            ]
                        ]
                    },
                    'SUPPORTS_FFD105' => 'N'
                ]
            ]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": true,
    "time": {
        "start": 1712135335.026931,
        "finish": 1712135335.407762,
        "duration": 0.3808310031890869,
        "processing": 0.0336611270904541,
        "date_start": "2024-04-03T11:08:55+02:00",
        "date_finish": "2024-04-03T11:08:55+02:00",
        "operating_reset_at": 1705765533,
        "operating": 3.3076241016387939
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../data-types.md) | Result of updating the REST cash register handler ||
|| **time**
[`time`](../../data-types.md) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**, **403**

```json
{
    "error": "ERROR_HANDLER_NOT_FOUND",
    "error_description": "Handler not found"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Status** ||
|| `ACCESS_DENIED` | Insufficient permissions to update the handler or the application is trying to modify a handler added by another application | 403 ||
|| `ERROR_CHECK_FAILURE` | The values for fields `ID` or `FIELDS` are not specified | 400 ||
|| `ERROR_HANDLER_NOT_FOUND` | Handler with the specified `ID` not found | 400 ||
|| `ERROR_HANDLER_UPDATE` | Other errors. More detailed information about the error can be found in `error_description` | 400 ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./sale-cashbox-handler-add.md)
- [{#T}](./sale-cashbox-handler-list.md)
- [{#T}](./sale-cashbox-handler-delete.md)
- [{#T}](./sale-cashbox-add.md)
- [{#T}](./sale-cashbox-update.md)
- [{#T}](./sale-cashbox-list.md)
- [{#T}](./sale-cashbox-delete.md)
- [{#T}](./sale-cashbox-check-apply.md)
