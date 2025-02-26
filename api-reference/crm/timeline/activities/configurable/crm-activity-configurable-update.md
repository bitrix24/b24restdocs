# Update Configurable Activity crm.activity.configurable.update

> Scope: [`crm`](../../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `crm.activity.configurable.update` makes changes to a configurable activity.

{% note warning %}

The method can only be called in the context of the [application](https://helpdesk.bitrix24.com/examples/app.zip) that created it.

{% endnote %}

## Method Parameters

{% include [Note on required parameters](../../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../../../data-types.md) | Integer identifier of the activity, for example `999` ||
|| **fields***
[`array`](../../../../data-types.md) | Associative array of values for [activity fields](./crm-activity-configurable-add.md#parametr-fields) in the following structure:

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
[`LayoutDto`](./structure/layout.md) | [Associative array of a special structure](./structure/layout.md#example), describing the appearance of the activity in the timeline ||
|#

## Code Examples

{% include [Note on examples](../../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":999,"fields":{"typeId":"CONFIGURABLE","completed":false,"deadline":"**put_current_date_time_here**","pingOffsets":[300],"isIncomingChannel":"Y","responsibleId":5,"badgeCode":"CUSTOM"},"layout":{"icon":{"code":"call-completed"},"header":{"title":"Incoming Call"},"body":{"logo":{"code":"call-incoming"},"blocks":{"responsible":{"type":"lineOfBlocks","properties":{"blocks":{"client":{"type":"link","properties":{"text":"John Smith","bold":true,"action":{"type":"redirect","uri":"/crm/lead/details/789/"}}},"phone":{"type":"text","properties":{"value":"+1 999 888 7777"}}}}}}},"footer":{"buttons":{"startCall":{"title":"About the Client","action":{"type":"openRestApp","actionParams":{"clientId":456}},"type":"primary"}}}},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.activity.configurable.update
    ```

- JS

    ```js
    BX24.callMethod(
        "crm.activity.configurable.update",
        {
            id: 999,
            fields:
            {
                typeId: 'CONFIGURABLE',
                completed: false,
                deadline: new Date(),
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
                            "title": "About the Client",
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

- PHP
  
    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.activity.configurable.update',
        [
            'id' => 999,
            'fields' => [
                'typeId' => 'CONFIGURABLE',
                'completed' => false,
                'deadline' => date('c'), // Using the current date and time in ISO 8601 format
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
                                }
                            ]
                        }
                    ]
                ],
                'footer' => [
                    'buttons' => [
                        'startCall' => [
                            'title' => 'About the Client',
                            'action' => [
                                'type' => 'openRestApp',
                                'actionParams' => [
                                    'clientId' => 456
                                ]
                            ],
                            'type' => 'primary'
                        }
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
            "id": 999
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
[`time`](../../../../data-types.md#time) | Information about the execution time of the request ||
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
|| `100` | Required fields are not filled ||
|| `ERROR_WRONG_CONTEXT` | Method call is only possible in the context of the application ||
|| `ERROR_WRONG_APPLICATION` | The activity can only be updated by the application that created it ||
|| `WRONG_FIELD_VALUE` | Incorrect field value ||
|| `INCOMING_ACTIVITY_CAN_NOT_BE_WITH_DEADLINE` | Incoming activity cannot have a deadline ||
|| `ERROR_EMPTY_LAYOUT` | The layout field must be filled ||
|#

{% include [system errors](../../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-activity-configurable-add.md)
- [{#T}](./crm-activity-configurable-get.md)