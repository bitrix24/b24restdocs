# Update Configurable Activity crm.activity.configurable.update

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../../../scopes/permissions.md)
>
> Who can execute the method: a user with edit access to the CRM object the activity is linked to

The `crm.activity.configurable.update` method updates the fields of a configurable activity, the structure of its timeline entry, or both at once.

The update is partial: fields not passed in `fields` keep their previous values. Without `layout` the entry keeps its current appearance. A `layout` that is passed replaces the previous structure as a whole, the fields are not merged.

The same method releases the [entry lock](./structure/action.md#sobytie) set by the `animationType` field of an action on click.

{% note info "" %}

The method can only be called within the context of the [application](../../../../../settings/app-installation/index.md) that created the activity. Calling it via an inbound webhook returns the `ERROR_WRONG_CONTEXT` error, calling it from another application — `ERROR_WRONG_APPLICATION`.

{% endnote %}

## Method Parameters

{% include [Note on required parameters](../../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../../../data-types.md) | Integer identifier of the activity, for example `999`. It is returned by the [crm.activity.configurable.add](./crm-activity-configurable-add.md) method ||
|| **fields**
[`array`](../../../../data-types.md) | Associative array of values for [activity fields](./crm-activity-configurable-add.md#parametr-fields) in the following structure:

```json
{
    "completed": true,
    "deadline": "2025-02-01T12:00:00+03:00",
    "pingOffsets": [15, 60],
    "isIncomingChannel": "N",
    "responsibleId": 5,
    "badgeCode": "CUSTOM",
    "originatorId": "my_service",
    "originId": "42"
}
```
The fields you pass are updated, the rest keep their previous values
||
|| **layout**
[`LayoutDto`](./structure/layout.md) | Structure that defines the appearance of the entry in the timeline. A ready-made [object example](./structure/layout.md#primer) is on the structure page ||
|#

Only `id` is required. The `fields` and `layout` parameters can be passed separately or together.

### How to Clear a Field Value

An empty value does not clear every field:

#|
|| **Field** | **How to Clear** ||
|| `badgeCode`, `originatorId`, `originId` | Pass an empty string ||
|| `pingOffsets` | Pass an empty array ||
|| `deadline` | Cannot be cleared through the method: Bitrix24 ignores an empty value and keeps the previous deadline ||
|#

The activity type `typeId` does not change after creation: any other value is rejected by the method with the `CANT_CHANGE_PROVIDER_TYPE_ID` error.

## Code Examples

{% include [Note on examples](../../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":999,"fields":{"completed":false,"deadline":"2025-02-01T12:00:00+03:00","pingOffsets":[300],"isIncomingChannel":"Y","responsibleId":5,"badgeCode":"CUSTOM"},"layout":{"icon":{"code":"call-completed"},"header":{"title":"Incoming call"},"body":{"logo":{"code":"call-incoming"},"blocks":{"responsible":{"type":"lineOfBlocks","properties":{"blocks":{"client":{"type":"link","properties":{"text":"Klaus Weber","bold":true,"action":{"type":"redirect","uri":"/crm/lead/details/789/"}}},"phone":{"type":"text","properties":{"value":"+49 999 888 7777"}}}}}}},"footer":{"buttons":{"startCall":{"title":"About the client","action":{"type":"openRestApp","actionParams":{"clientId":456}},"type":"primary"}}}},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.activity.configurable.update
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type ActivityUpdateResult = {
      activity: {
        id: number
      }
    }

    try {
      const response = await $b24.actions.v2.call.make<ActivityUpdateResult>({
        method: 'crm.activity.configurable.update',
        params: {
          id: 999,
          fields: {
            completed: false,
            deadline: '2025-02-01T12:00:00+03:00',
            pingOffsets: [300],
            isIncomingChannel: 'Y',
            responsibleId: 5,
            badgeCode: 'CUSTOM',
          },
          layout: {
            icon: {
              code: 'call-completed',
            },
            header: {
              title: 'Incoming call',
            },
            body: {
              logo: {
                code: 'call-incoming',
              },
              blocks: {
                responsible: {
                  type: 'lineOfBlocks',
                  properties: {
                    blocks: {
                      client: {
                        type: 'link',
                        properties: {
                          text: 'John Smith',
                          bold: true,
                          action: {
                            type: 'redirect',
                            uri: '/crm/lead/details/789/',
                          },
                        },
                      },
                      phone: {
                        type: 'text',
                        properties: {
                          value: '+49 999 888 7777',
                        },
                      },
                    },
                  },
                },
              },
            },
            footer: {
              buttons: {
                startCall: {
                  title: 'About client',
                  action: {
                    type: 'openRestApp',
                    actionParams: {
                      clientId: 456,
                    },
                  },
                  type: 'primary',
                },
              },
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
        console.info('Updated activity id:', result.activity.id)
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
      async function updateConfigurableActivity() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'crm.activity.configurable.update',
            params: {
              id: 999,
              fields: {
                completed: false,
                deadline: '2025-02-01T12:00:00+03:00',
                pingOffsets: [300],
                isIncomingChannel: 'Y',
                responsibleId: 5,
                badgeCode: 'CUSTOM',
              },
              layout: {
                icon: {
                  code: 'call-completed',
                },
                header: {
                  title: 'Incoming call',
                },
                body: {
                  logo: {
                    code: 'call-incoming',
                  },
                  blocks: {
                    responsible: {
                      type: 'lineOfBlocks',
                      properties: {
                        blocks: {
                          client: {
                            type: 'link',
                            properties: {
                              text: 'John Smith',
                              bold: true,
                              action: {
                                type: 'redirect',
                                uri: '/crm/lead/details/789/',
                              },
                            },
                          },
                          phone: {
                            type: 'text',
                            properties: {
                              value: '+49 999 888 7777',
                            },
                          },
                        },
                      },
                    },
                  },
                },
                footer: {
                  buttons: {
                    startCall: {
                      title: 'About client',
                      action: {
                        type: 'openRestApp',
                        actionParams: {
                          clientId: 456,
                        },
                      },
                      type: 'primary',
                    },
                  },
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
          console.info('Updated activity id:', result.activity.id)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', updateConfigurableActivity)
    </script>
    ```

- Python

    ```python

    # The b24pysdk wrapper accepts only fields — it updates the activity fields.
    # To replace the entry structure, call the method directly and pass layout.

    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.crm.activity.configurable.update(
            bitrix_id=999,
            fields={
                "completed": False,
                "deadline": "2025-02-01T12:00:00+03:00",
                "pingOffsets": [300],
                "responsibleId": 5,
                "badgeCode": "CUSTOM",
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

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.activity.configurable.update',
                [
                    'id'     => 999,
                    'fields' => [
                        'completed'         => false,
                        'deadline'          => '2025-02-01T12:00:00+03:00',
                        'pingOffsets'       => [300],
                        'isIncomingChannel' => 'Y',
                        'responsibleId'     => 5,
                        'badgeCode'         => 'CUSTOM',
                    ],
                    'layout' => [
                        'icon'   => [
                            'code' => 'call-completed',
                        ],
                        'header' => [
                            'title' => 'Incoming call',
                        ],
                        'body'   => [
                            'logo'   => [
                                'code' => 'call-incoming',
                            ],
                            'blocks' => [
                                'responsible' => [
                                    'type'       => 'lineOfBlocks',
                                    'properties' => [
                                        'blocks' => [
                                            'client' => [
                                                'type'       => 'link',
                                                'properties' => [
                                                    'text'   => 'Klaus Weber',
                                                    'bold'   => true,
                                                    'action' => [
                                                        'type' => 'redirect',
                                                        'uri'  => '/crm/lead/details/789/',
                                                    ],
                                                ],
                                            ],
                                            'phone'  => [
                                                'type'       => 'text',
                                                'properties' => [
                                                    'value' => '+49 999 888 7777',
                                                ],
                                            ],
                                        ],
                                    ],
                                ],
                            ],
                        ],
                        'footer' => [
                            'buttons' => [
                                'startCall' => [
                                    'title'  => 'About the client',
                                    'action' => [
                                        'type'         => 'openRestApp',
                                        'actionParams' => [
                                            'clientId' => 456,
                                        ],
                                    ],
                                    'type'   => 'primary',
                                ],
                            ],
                        ],
                    ],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
        // The data processing logic you need
        processData($result);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error updating configurable activity: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "crm.activity.configurable.update",
        {
            id: 999,
            fields:
            {
                completed: false,
                deadline: '2025-02-01T12:00:00+03:00',
                pingOffsets: [300],
                isIncomingChannel: 'Y',
                responsibleId: 5,
                badgeCode: 'CUSTOM',
            },
            layout:
            {
                "icon": {
                    "code": "call-completed"
                },
                "header": {
                    "title": "Incoming call"
                },
                "body": {
                    "logo": {
                        "code": "call-incoming"
                    },
                    "blocks": {
                        "responsible": {
                            "type": "lineOfBlocks",
                            "properties": {
                                "blocks": {
                                    "client": {
                                        "type": "link",
                                        "properties": {
                                            "text": "Klaus Weber",
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
                                            "value": "+49 999 888 7777"
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
                            "title": "About the client",
                            "action": {
                                "type": "openRestApp",
                                "actionParams": {
                                    "clientId": 456
                                }
                            },
                            "type": "primary"
                        }
                    }
                }
            }
        }, result => {
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
        'crm.activity.configurable.update',
        [
            'id' => 999,
            'fields' => [
                'completed' => false,
                'deadline' => '2025-02-01T12:00:00+03:00',
                'pingOffsets' => [300],
                'isIncomingChannel' => 'Y',
                'responsibleId' => 5,
                'badgeCode' => 'CUSTOM',
            ],
            'layout' => [
                'icon' => [
                    'code' => 'call-completed'
                ],
                'header' => [
                    'title' => 'Incoming call'
                ],
                'body' => [
                    'logo' => [
                        'code' => 'call-incoming'
                    ],
                    'blocks' => [
                        'responsible' => [
                            'type' => 'lineOfBlocks',
                            'properties' => [
                                'blocks' => [
                                    'client' => [
                                        'type' => 'link',
                                        'properties' => [
                                            'text' => 'Klaus Weber',
                                            'bold' => true,
                                            'action' => [
                                                'type' => 'redirect',
                                                'uri' => '/crm/lead/details/789/'
                                            ]
                                        ]
                                    ],
                                    'phone' => [
                                        'type' => 'text',
                                        'properties' => [
                                            'value' => '+49 999 888 7777'
                                        ]
                                    ]
                                ]
                            ]
                        ]
                    ]
                ],
                'footer' => [
                    'buttons' => [
                        'startCall' => [
                            'title' => 'About the client',
                            'action' => [
                                'type' => 'openRestApp',
                                'actionParams' => [
                                    'clientId' => 456
                                ]
                            ],
                            'type' => 'primary'
                        ]
                    ]
                ]
            ]
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
    res, err := client.Core().Call(ctx, "crm.activity.configurable.update", b24.Params{
    	"id": 999,
    	"fields": b24.Params{
    		"completed":         false,
    		"deadline":          "2025-02-01T12:00:00+03:00",
    		"pingOffsets":       []int{300},
    		"isIncomingChannel": "Y",
    		"responsibleId":     5,
    		"badgeCode":         "CUSTOM",
    	},
    	"layout": b24.Params{
    		"icon": b24.Params{
    			"code": "call-completed",
    		},
    		"header": b24.Params{
    			"title": "Incoming call",
    		},
    		"body": b24.Params{
    			"logo": b24.Params{
    				"code": "call-incoming",
    			},
    			"blocks": b24.Params{
    				"responsible": b24.Params{
    					"type": "lineOfBlocks",
    					"properties": b24.Params{
    						"blocks": b24.Params{
    							"client": b24.Params{
    								"type": "link",
    								"properties": b24.Params{
    									"text": "Klaus Weber",
    									"bold": true,
    									"action": b24.Params{
    										"type": "redirect",
    										"uri":  "/crm/lead/details/789/",
    									},
    								},
    							},
    							"phone": b24.Params{
    								"type": "text",
    								"properties": b24.Params{
    									"value": "+49 999 888 7777",
    								},
    							},
    						},
    					},
    				},
    			},
    		},
    		"footer": b24.Params{
    			"buttons": b24.Params{
    				"startCall": b24.Params{
    					"title": "About the client",
    					"action": b24.Params{
    						"type": "openRestApp",
    						"actionParams": b24.Params{
    							"clientId": 456,
    						},
    					},
    					"type": "primary",
    				},
    			},
    		},
    	},
    })
    if err != nil {
    	return fmt.Errorf("crm.activity.configurable.update: %w", err)
    }

    // The response arrives as json.RawMessage — unmarshal it
    // into a struct matching the response shape shown below on this page.
    fmt.Printf("%s\n", res.Result)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "activity": {
            "id": 999
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
[`integer`](../../../../data-types.md) | Identifier of the updated activity ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "ERROR_WRONG_APPLICATION",
    "error_description": "The activity can only be updated by the application that created it"
}
```

{% include notitle [Error handling](../../../../../_includes/error-info.md) %}

### Possible Error Codes {#errors}

#|
|| **Code** | **Description** ||
|| `ACCESS_DENIED` | Insufficient permissions to update the activity ||
|| `NOT_FOUND` | The activity was not found or is not configurable ||
|| `100` | The required `id` parameter is missing ||
|| `ERROR_WRONG_CONTEXT` | The method was called outside an application, for example via an inbound webhook ||
|| `ERROR_WRONG_APPLICATION` | The activity can only be updated by the application that created it ||
|| `WRONG_FIELD_VALUE` | Incorrect field value: an unknown `badgeCode` or `typeId` in `fields`, an unsupported nested block type, an invalid color format in `sliderParams` ||
|| `INCOMING_ACTIVITY_CAN_NOT_BE_WITH_DEADLINE` | Incoming activity cannot have a deadline ||
|| `CANT_CHANGE_PROVIDER_TYPE_ID` | The activity type cannot be changed: the `typeId` passed differs from the type the activity was created with ||
|| `FIELD_IS_REQUIRED` | A required field was not passed in the structure object ||
|| `FIELD_IS_REDUNDANT` | A field was passed in the structure object that is not in its description ||
|| `ENUM_FIELD` | The field value is not in the list of allowed values, for example, an unknown tag type ||
|| `TOO_MANY_ITEMS` | The number of array elements has been exceeded, for example, more than two tags or buttons ||
|| `KEY_CONTAIN_WRONG_SYMBOLS` | The key in the structure's associative array contains invalid characters. Only Latin letters, digits, hyphens, and underscores are allowed ||
|| `WRONG_LANG` | In a multi-language value, a language code was passed that is not installed in Bitrix24 ||
|#

{% include [System errors](../../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-activity-configurable-add.md)
- [{#T}](./crm-activity-configurable-get.md)
- [{#T}](./structure/layout.md)
- [{#T}](./structure/examples.md)
- [{#T}](./badges/index.md)
- [{#T}](../activity-base/crm-activity-list.md)
- [{#T}](../activity-base/crm-activity-delete.md)
- [{#T}](./index.md)
