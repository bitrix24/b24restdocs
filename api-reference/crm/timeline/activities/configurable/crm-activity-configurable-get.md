# Get Configurable Activity crm.activity.configurable.get

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../../../scopes/permissions.md)
>
> Who can execute the method: a user with read access to the CRM object the activity is linked to

The method `crm.activity.configurable.get` returns a configurable activity by its identifier: its fields and the `layout` structure that defines the appearance of the entry in the timeline.

Unlike [crm.activity.configurable.add](./crm-activity-configurable-add.md) and [crm.activity.configurable.update](./crm-activity-configurable-update.md), the method does not require the application context — it can also be called via an inbound webhook.

The method returns only configurable activities — for activities of other types it returns `NOT_FOUND`.

The activity identifier comes in the response of the [crm.activity.configurable.add](./crm-activity-configurable-add.md) method. You can find the activities created by the application with the [crm.activity.list](../activity-base/crm-activity-list.md) method using the `PROVIDER_ID = CONFIGURABLE_REST_APP` filter.

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

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":999}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.activity.configurable.get
    ```

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

- Python

    ```python

    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

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
    
        echo 'Data: ' . print_r($result, true);
    
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

    if (isset($result['error'])) {
        echo 'Error: ' . $result['error_description'];
    } else {
        echo '<PRE>';
        print_r($result['result']);
        echo '</PRE>';
    }
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "crm.activity.configurable.get", b24.Params{
    	"id": 999,
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("crm.activity.configurable.get: %w", err)
    }

    // The response arrives as json.RawMessage — unmarshal it
    // into a struct matching the response shape shown below on this page.
    fmt.Printf("%s\n", res.Result)
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
[`object`](../../../../data-types.md) | Root element of the response with a single **activity** key [(detailed description)](#activity) ||
|| **time**
[`time`](../../../../data-types.md#time) | Information about the request execution time ||
|#

#### Activity Object {#activity}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../../../data-types.md) | Identifier of the activity ||
|| **ownerTypeId**
[`integer`](../../../../data-types.md) | Identifier of the [CRM object type](../../../data-types.md#object_type) the activity is linked to ||
|| **ownerId**
[`integer`](../../../../data-types.md) | Identifier of the CRM object the activity is linked to ||
|| **fields**
[`object`](../../../../data-types.md) | Activity fields [(detailed description)](#fields) ||
|| **layout**
[`LayoutDto`](./structure/layout.md) | Structure that defines the appearance of the entry in the timeline. The same structure is passed in the `layout` parameter of the [crm.activity.configurable.add](./crm-activity-configurable-add.md) and [crm.activity.configurable.update](./crm-activity-configurable-update.md) methods. The `actionParams` values inside the structure come back as strings even if they were passed as numbers on creation: in the example above `clientId` came back as `"456"` ||
|#

#### Fields Object {#fields}

The types in the response differ from the input types. Flags come back as `boolean` even if they were passed as `Y/N` or `1/0` on creation. Unset values come back as `null` or an empty string.

#|
|| **Name**
`type` | **Description** ||
|| **typeId**
[`string`](../../../../data-types.md) | Type of the configurable activity, for example `CONFIGURABLE` ||
|| **completed**
[`boolean`](../../../../data-types.md) | Whether the activity is closed ||
|| **deadline**
[`datetime`](../../../../data-types.md) | Deadline in ISO 8601 format, or `null` if there is no deadline ||
|| **pingOffsets**
[`array`](../../../../data-types.md) | Offsets in minutes relative to the deadline. An empty array if no pings are set ||
|| **isIncomingChannel**
[`boolean`](../../../../data-types.md) | Whether the activity was created from an incoming channel ||
|| **responsibleId**
[`integer`](../../../../data-types.md) | Identifier of the assignee ||
|| **badgeCode**
[`string`](../../../../data-types.md) | [Badge](./badges/index.md) code, or an empty string if no badge is set ||
|| **originatorId**
[`string`](../../../../data-types.md) | Identifier of the data source, or `null` ||
|| **originId**
[`string`](../../../../data-types.md) | Identifier of the element in the data source, or `null` ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "NOT_FOUND",
    "error_description": "Element not found"
}
```

{% include notitle [error handling](../../../../../_includes/error-info.md) %}

### Possible Error Codes {#errors}

#|
|| **Code** | **Description** ||
|| `100` | The required `id` parameter is missing ||
|| `NOT_FOUND` | The activity was not found: no such identifier, the activity is not configurable, or the user has no access to the CRM object ||
|#

{% include [system errors](../../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-activity-configurable-add.md)
- [{#T}](./crm-activity-configurable-update.md)
- [{#T}](./structure/layout.md)
- [{#T}](./structure/examples.md)
- [{#T}](./badges/index.md)
- [{#T}](../activity-base/crm-activity-list.md)
- [{#T}](../activity-base/crm-activity-delete.md)
- [{#T}](./index.md)
