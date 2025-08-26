# Add Configurable Activity crm.activity.configurable.add

> Scope: [`crm`](../../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `crm.activity.configurable.add` adds a configurable activity to the timeline.

{% note warning %}

The method can only be called in the context of an [application](https://helpdesk.bitrix24.com/examples/app.zip).

{% endnote %}

## Method Parameters

{% include [Note on required parameters](../../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **ownerTypeId***
[`integer`](../../../../data-types.md) | Integer identifier of the [CRM object type](../../../data-types.md#object_type) where the activity is created, for example `2` for a deal ||
|| **ownerId***
[`integer`](../../../../data-types.md) | Integer identifier of the CRM element where the activity is created, for example `1` ||
|| **fields***
[`array`](../../../../data-types.md) | Associative array of values for [activity fields](#parametr-fields) in the following structure:
```json
fields:
{
    "typeId": 'value',
    "completed": 'value',
    "deadline": 'value',
    "pingOffsets": 'value',
    "isIncomingChannel": 'value',
    "responsibleId": 'value',
    "badgeCode": 'value',
    "originatorId": 'value',
    "originId": 'value',
}
```
||
|| **layout***
[`LayoutDto`](./structure/layout.md) | [Associative array of a special structure](./structure/layout.md#example) describing the appearance of the activity in the timeline ||
|#

### Parameter fields {#parametr-fields}

{% include [Note on required parameters](../../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **typeId**
[`string`](../../../../data-types.md) | Type of the configurable activity. If the value is not specified, it defaults to `CONFIGURABLE`. If specified, the value must correspond to one of the types created by the method [crm.activity.type.add](../types/crm-activity-type-add.md) with the field `IS_CONFIGURABLE_TYP0` equal to `Y` in the context of the same application ||
|| **completed**
[`boolean`](../../../../data-types.md) | Flag indicating whether the activity is closed. You can use `Y/N`, `1/0`, `true/false` to set the value ||
|| **deadline**
[`datetime`](../../../../data-types.md) | Deadline for the activity ||
|| **pingOffsets**
[`array`](../../../../data-types.md) | Array of offsets in seconds relative to the deadline, determining when to create ping records for this activity ||
|| **isIncomingChannel**
[`boolean`](../../../../data-types.md) | Flag indicating whether the activity was created from an incoming channel. You can use `Y/N`, `1/0`, `true/false` to set the value ||
|| **responsibleId**
[`integer`](../../../../data-types.md) | Responsible person for the activity ||
|| **badgeCode**
[`string`](../../../../data-types.md) | Code of the [badge on the kanban](./badges/crm-activity-badge-list.md) corresponding to the activity ||
|| **originatorId**
[`string`](../../../../data-types.md) | Identifier of the data source ||
|| **originId**
[`string`](../../../../data-types.md) | Identifier of the element in the data source ||
|#

## Code Examples

{% include [Note on examples](../../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"ownerTypeId":1,"ownerId":999,"fields":{"typeId":"CONFIGURABLE","completed":true,"deadline":"**put_current_date_time_here**","pingOffsets":[60,300],"isIncomingChannel":"N","responsibleId":1,"badgeCode":"CUSTOM"},"layout":{"icon":{"code":"call-completed"},"header":{"title":"Incoming Call"},"body":{"logo":{"code":"call-incoming"},"blocks":{"responsible":{"type":"lineOfBlocks","properties":{"blocks":{"client":{"type":"link","properties":{"text":"John Smith","bold":true,"action":{"type":"redirect","uri":"/crm/lead/details/789/"}}},"phone":{"type":"text","properties":{"value":"+1 999 888 7777"}}}}}}},"footer":{"buttons":{"startCall":{"title":"About Client","action":{"type":"openRestApp","actionParams":{"clientId":456}},"type":"primary"}}}},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.activity.configurable.add
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		"crm.activity.configurable.add",
    		{
    			ownerTypeId: 1,
    			ownerId: 999,
    			fields:
    			{
    				typeId: 'CONFIGURABLE',
    				completed: true,
    				deadline: new Date(),
    				pingOffsets: [60, 300],
    				isIncomingChannel: 'N',
    				responsibleId: 1,
    				badgeCode: 'CUSTOM',
    			},
    			layout:
    			{
    				"icon": {
    					"code": "call-completed"
    				},
    				"header": {
    					"title": "Incoming Call"
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
    											"text": "John Smith",
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
    		}
    	);
    	
    	const result = response.getData().result;
    	console.dir(result);
    }
    catch( error )
    {
    	console.error(error);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.activity.configurable.add',
                [
                    'ownerTypeId' => 1,
                    'ownerId' => 999,
                    'fields' => [
                        'typeId' => 'CONFIGURABLE',
                        'completed' => true,
                        'deadline' => new DateTime(),
                        'pingOffsets' => [60, 300],
                        'isIncomingChannel' => 'N',
                        'responsibleId' => 1,
                        'badgeCode' => 'CUSTOM',
                    ],
                    'layout' => [
                        'icon' => [
                            'code' => 'call-completed',
                        ],
                        'header' => [
                            'title' => 'Incoming Call',
                        ],
                        'body' => [
                            'logo' => [
                                'code' => 'call-incoming',
                            ],
                            'blocks' => [
                                'responsible' => [
                                    'type' => 'lineOfBlocks',
                                    'properties' => [
                                        'blocks' => [
                                            'client' => [
                                                'type' => 'link',
                                                'properties' => [
                                                    'text' => 'John Smith',
                                                    'bold' => true,
                                                    'action' => [
                                                        'type' => 'redirect',
                                                        'uri' => '/crm/lead/details/789/',
                                                    ],
                                                ],
                                            ],
                                            'phone' => [
                                                'type' => 'text',
                                                'properties' => [
                                                    'value' => '+1 999 888 7777',
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
                                    'title' => 'About Client',
                                    'action' => [
                                        'type' => 'openRestApp',
                                        'actionParams' => [
                                            'clientId' => 456,
                                        ],
                                    ],
                                    'type' => 'primary',
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
        // Your logic for processing data
        processData($result);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error adding configurable activity: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "crm.activity.configurable.add",
        {
            ownerTypeId: 1,
            ownerId: 999,
            fields:
            {
                typeId: 'CONFIGURABLE',
                completed: true,
                deadline: new Date(),
                pingOffsets: [60, 300],
                isIncomingChannel: 'N',
                responsibleId: 1,
                badgeCode: 'CUSTOM',
            },
            layout:
            {
                "icon": {
                    "code": "call-completed"
                },
                "header": {
                    "title": "Incoming Call"
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
                                            "text": "John Smith",
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
        'crm.activity.configurable.add',
        [
            'ownerTypeId' => 1,
            'ownerId' => 999,
            'fields' => [
                'typeId' => 'CONFIGURABLE',
                'completed' => true,
                'deadline' => date('c'), // Using the current date and time in ISO 8601 format
                'pingOffsets' => [60, 300],
                'isIncomingChannel' => 'N',
                'responsibleId' => 1,
                'badgeCode' => 'CUSTOM',
            ],
            'layout' => [
                'icon' => [
                    'code' => 'call-completed'
                ],
                'header' => [
                    'title' => 'Incoming Call'
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
                                            'text' => 'John Smith',
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
                                            'value' => '+1 999 888 7777'
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
                            'title' => 'About Client',
                            'action' => [
                                'type' => 'openRestApp',
                                'actionParams' => [
                                    'clientId' => 456
                                ]
                            },
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

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": {
		"activity": {
			"id": 999,
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
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../../data-types.md) | Root element of the response containing information about the added activity identifier `id` in case of success. In case of failure, it will return `null` ||
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
|| `100` | Required fields are not filled ||
|| `ERROR_WRONG_CONTEXT` | The method can only be called in the context of an application ||
|| `ERROR_WRONG_APPLICATION` | The activity can only be updated by the application that created it ||
|| `WRONG_FIELD_VALUE` | Incorrect field value ||
|| `INCOMING_ACTIVITY_CAN_NOT_BE_WITH_DEADLINE` | Incoming activity cannot have a deadline ||
|| `ERROR_EMPTY_LAYOUT` | The layout field must be filled ||
|#

{% include [system errors](../../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-activity-configurable-update.md)
- [{#T}](./crm-activity-configurable-get.md)