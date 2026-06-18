# Get Configurable Activity by ID crm.activity.configurable.get

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`crm`](../../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `crm.activity.configurable.get` returns information about a configurable activity.

{% note warning %}

The method can only be called in the context of an [application](https://helpdesk.bitrix24.com/examples/app.zip).

{% endnote %}

## Method Parameters

{% include [Note on required parameters](../../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../../../data-types.md) | Integer identifier of the activity, for example `999` ||
|#

## Code Examples

{% include [Note on examples](../../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":999,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.activity.configurable.get
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame, ISODate } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type ConfigurableActivityGetResult = {
      activity: {
        id: number
        ownerTypeId: number
        ownerId: number
        fields: {
          typeId: string
          completed: boolean
          deadline: ISODate | null
          pingOffsets: number[]
          isIncomingChannel: boolean
          responsibleId: number
          badgeCode: string
          originatorId: string | null
          originId: string | null
        }
        layout: Record<string, unknown>
      }
    }

    try {
      const response = await $b24.actions.v2.call.make<ConfigurableActivityGetResult>({
        method: 'crm.activity.configurable.get',
        params: {
          id: 999,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info(result.activity.id, result.activity.fields.typeId, result.activity.layout)
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
      async function getConfigurableActivity() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'crm.activity.configurable.get',
            params: {
              id: 999,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info(result.activity.id, result.activity.fields.typeId, result.activity.layout)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getConfigurableActivity)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.activity.configurable.get',
                [
                    'id' => 999,
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        if ($result->error()) {
            error_log($result->error());
        } else {
            echo 'Data: ' . print_r($result->data(), true);
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting configurable activity: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "crm.activity.configurable.get",
        {
            id: 999,
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

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.activity.configurable.get',
        [
            'id' => 999
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Python

    Example

    ```python
    from b24pysdk.client import BaseClient
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    client: BaseClient

    try:
        bitrix_response = client.crm.activity.configurable.get(
            bitrix_id=999,
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
{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": {
        "activity": {
            "id": 8903,
            "ownerTypeId": 1,
            "ownerId": 2975,
            "fields": {
                "typeId": "CONFIGURABLE",
                "completed": false,
                "deadline": "2025-02-01T01:00:00+03:00",
                "pingOffsets": [],
                "isIncomingChannel": false,
                "responsibleId": 1,
                "badgeCode": "",
                "originatorId": null,
                "originId": null
            },
            "layout": {
                "icon": {
                    "code": "call-completed"
                },
                "header": {
                    "title": "Incoming Call",
                    "tags": {
                        "status2": {
                            "title": "not transcribed",
                            "type": "warning"
                        }
                    }
                },
                "body": {
                    "logo": {
                        "code": "call-incoming",
                        "action": {
                            "type": "redirect",
                            "uri": "/crm/deal/details/123/"
                        }
                    },
                    "blocks": {
                        "client": {
                            "type": "withTitle",
                            "properties": {
                                "title": "Client",
                                "inline": true,
                                "block": {
                                    "type": "text",
                                    "properties": {
                                        "value": "Ltd. Hoofs and Horns"
                                    }
                                }
                            }
                        },
                        "responsible": {
                            "type": "lineOfBlocks",
                            "properties": {
                                "blocks": {
                                    "client": {
                                        "type": "link",
                                        "properties": {
                                            "text": "Sergey Vostrikov",
                                            "bold": true,
                                            "action": {
                                                "type": "redirect",
                                                "uri": "/crm/lead/details/789/"
                                            }
                                        }
                                    },
                                    "phone": {
                                        "type": "text",
                                        "properties": {
                                            "value": "+1 999 888 7777"
                                        }
                                    }
                                }
                            }
                        }
                    }
                },
                "footer": {
                    "buttons": {
                        "startCall": {
                            "title": "About Client",
                            "type": "primary",
                            "action": {
                                "type": "openRestApp",
                                "actionParams": {
                                    "clientId": "456"
                                }
                            }
                        }
                    },
                    "menu": {
                        "showPostponeItem": false,
                        "items": {
                            "confirm": {
                                "title": "Confirm Request",
                                "action": {
                                    "type": "restEvent",
                                    "id": "confirm",
                                    "animationType": "loader"
                                }
                            },
                            "decline": {
                                "title": "Decline Request",
                                "action": {
                                    "type": "restEvent",
                                    "id": "decline",
                                    "animationType": "loader"
                                }
                            }
                        }
                    }
                }
            }
        }
    },
    "time": {
        "start": 1724068028.331234,
        "finish": 1724068028.726591,
        "duration": 0.3953571319580078,
        "processing": 0.13033390045166016,
        "date_start": "2025-01-21T13:47:08+02:00",
        "date_finish": "2025-01-21T13:47:08+02:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../../data-types.md) | Root element of the response - an associative array with the key **activity**, which will contain [fields](./crm-activity-configurable-add.md#parametr-fields) ||
|| **time**
[`time`](../../../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "NOT_FOUND",
    "error_description": "Not found."
}
```

{% include notitle [error handling](../../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `ACCESS_DENIED` | Insufficient permissions to perform the operation ||
|| `NOT_FOUND` | Element not found ||
|| `ERROR_WRONG_CONTEXT` | Method call is only possible in the context of an application ||
|#

{% include [system errors](../../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-activity-configurable-update.md)
- [{#T}](./crm-activity-configurable-add.md)