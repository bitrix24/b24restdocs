# Send a message to a user on behalf of the Open Channel imopenlines.network.message.add

> Scope: [`imopenlines`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `imopenlines.network.message.add` sends a message to a user on behalf of the open channel connected in Bitrix24 Network.

Method operation limitations:
1. The method is unavailable during session authorization. It returns the error `WRONG_AUTH_TYPE` for session authorization.
2. A message can be sent no more than once per user within one week. There are no limits for accounts with a Partner (NFR) license.
3. The keyboard can only be used for formatting the link button to an external site.

## Method Parameters

{% include [Footnote on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **CODE***
[`string`](../../data-types.md) | Code of the open channel, a string of 32 characters, for example `ab515f5d85a8b844d484f6ea75a2e494` ||
|| **USER_ID***
[`integer`](../../data-types.md) | Identifier of the message recipient, for example `2` ||
|| **MESSAGE***
[`string`](../../data-types.md) | Text of the message. 

How to format text — in the article [formatting](../../chats/messages/formatting.md) ||
|| **ATTACH**
[`object`](../../data-types.md) | Attachment.

The format of the attachment is described in the article [How to use attachments](../../chats/messages/attachments.md) ||
|| **KEYBOARD**
[`object`](../../data-types.md) | Keyboard.

How to create keyboards — in the article [Working with keyboards](../../chats/messages/keyboards.md) ||
|| **URL_PREVIEW**
[`char`](../../data-types.md) | Link preview. Enabled `Y` by default. 

Pass `N` to disable link preview ||
|#

## Code Examples

{% include [Footnote on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{
        "CODE": "ab515f5d85a8b844d484f6ea75a2e494",
        "USER_ID": 2,
        "MESSAGE": "We have prepared materials for connecting open channels",
        "ATTACH": {
          "ID": 1,
          "COLOR_TOKEN": "primary",
          "BLOCKS": [
            {
              "MESSAGE": "We have sent a checklist and connection diagram in the attachment"
            },
            {
              "FILE": [
                {
                  "NAME": "checklist-openlines.pdf",
                  "LINK": "https://cdn.example.com/docs/checklist-openlines.pdf",
                  "SIZE": 428736
                }
              ]
            },
            {
              "IMAGE": [
                {
                  "NAME": "Connection Diagram",
                  "LINK": "https://cdn.example.com/images/openlines-setup.png",
                  "PREVIEW": "https://cdn.example.com/images/openlines-setup-preview.png",
                  "WIDTH": 1280,
                  "HEIGHT": 720
                }
              ]
            }
          ]
        },
        "KEYBOARD": {
          "BUTTONS": [
            {
              "TEXT": "Open Instructions",
              "LINK": "https://help.example.com/openlines/setup",
              "DISPLAY": "LINE",
              "BG_COLOR_TOKEN": "primary"
            }
          ]
        },
        "URL_PREVIEW": "N"
      }' \
      https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/imopenlines.network.message.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{
        "CODE": "ab515f5d85a8b844d484f6ea75a2e494",
        "USER_ID": 2,
        "MESSAGE": "We have prepared materials for connecting open channels",
        "ATTACH": {
          "ID": 1,
          "COLOR_TOKEN": "primary",
          "BLOCKS": [
            {
              "MESSAGE": "We have sent a checklist and connection diagram in the attachment"
            },
            {
              "FILE": [
                {
                  "NAME": "checklist-openlines.pdf",
                  "LINK": "https://cdn.example.com/docs/checklist-openlines.pdf",
                  "SIZE": 428736
                }
              ]
            },
            {
              "IMAGE": [
                {
                  "NAME": "Connection Diagram",
                  "LINK": "https://cdn.example.com/images/openlines-setup.png",
                  "PREVIEW": "https://cdn.example.com/images/openlines-setup-preview.png",
                  "WIDTH": 1280,
                  "HEIGHT": 720
                }
              ]
            }
          ]
        },
        "KEYBOARD": {
          "BUTTONS": [
            {
              "TEXT": "Open Instructions",
              "LINK": "https://help.example.com/openlines/setup",
              "DISPLAY": "LINE",
              "BG_COLOR_TOKEN": "primary"
            }
          ]
        },
        "URL_PREVIEW": "N",
        "auth": "**put_access_token_here**"
      }' \
      https://**put_your_bitrix24_address**/rest/imopenlines.network.message.add
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'imopenlines.network.message.add',
            {
                CODE: 'ab515f5d85a8b844d484f6ea75a2e494',
                USER_ID: 2,
                MESSAGE: 'We have prepared materials for connecting open channels',
                ATTACH: {
                    ID: 1,
                    COLOR_TOKEN: 'primary',
                    BLOCKS: [
                        {
                            MESSAGE: 'We have sent a checklist and connection diagram in the attachment'
                        },
                        {
                            FILE: [
                                {
                                    NAME: 'checklist-openlines.pdf',
                                    LINK: 'https://cdn.example.com/docs/checklist-openlines.pdf',
                                    SIZE: 428736
                                }
                            ]
                        },
                        {
                            IMAGE: [
                                {
                                    NAME: 'Connection Diagram',
                                    LINK: 'https://cdn.example.com/images/openlines-setup.png',
                                    PREVIEW: 'https://cdn.example.com/images/openlines-setup-preview.png',
                                    WIDTH: 1280,
                                    HEIGHT: 720
                                }
                            ]
                        }
                    ]
                },
                KEYBOARD: {
                    BUTTONS: [
                        {
                            TEXT: 'Open Instructions',
                            LINK: 'https://help.example.com/openlines/setup',
                            DISPLAY: 'LINE',
                            BG_COLOR_TOKEN: 'primary'
                        }
                    ]
                },
                URL_PREVIEW: 'N'
            }
        );

        const result = response.getData().result;
        console.log(result);
    }
    catch (error)
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
                'imopenlines.network.message.add',
                [
                    'CODE' => 'ab515f5d85a8b844d484f6ea75a2e494',
                    'USER_ID' => 2,
                    'MESSAGE' => 'We have prepared materials for connecting open channels',
                    'ATTACH' => [
                        'ID' => 1,
                        'COLOR_TOKEN' => 'primary',
                        'BLOCKS' => [
                            [
                                'MESSAGE' => 'We have sent a checklist and connection diagram in the attachment'
                            ],
                            [
                                'FILE' => [
                                    [
                                        'NAME' => 'checklist-openlines.pdf',
                                        'LINK' => 'https://cdn.example.com/docs/checklist-openlines.pdf',
                                        'SIZE' => 428736
                                    ]
                                ]
                            ],
                            [
                                'IMAGE' => [
                                    [
                                        'NAME' => 'Connection Diagram',
                                        'LINK' => 'https://cdn.example.com/images/openlines-setup.png',
                                        'PREVIEW' => 'https://cdn.example.com/images/openlines-setup-preview.png',
                                        'WIDTH' => 1280,
                                        'HEIGHT' => 720
                                    ]
                                ]
                            ]
                        ]
                    ],
                    'KEYBOARD' => [
                        'BUTTONS' => [
                            [
                                'TEXT' => 'Open Instructions',
                                'LINK' => 'https://help.example.com/openlines/setup',
                                'DISPLAY' => 'LINE',
                                'BG_COLOR_TOKEN' => 'primary'
                            ]
                        ]
                    ],
                    'URL_PREVIEW' => 'N',
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        print_r($result);
    } catch (Throwable $e) {
        echo $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'imopenlines.network.message.add',
        {
            CODE: 'ab515f5d85a8b844d484f6ea75a2e494',
            USER_ID: 2,
            MESSAGE: 'We have prepared materials for connecting open channels',
            ATTACH: {
                ID: 1,
                COLOR_TOKEN: 'primary',
                BLOCKS: [
                    {
                        MESSAGE: 'We have sent a checklist and connection diagram in the attachment'
                    },
                    {
                        FILE: [
                            {
                                NAME: 'checklist-openlines.pdf',
                                LINK: 'https://cdn.example.com/docs/checklist-openlines.pdf',
                                SIZE: 428736
                            }
                        ]
                    },
                    {
                        IMAGE: [
                            {
                                NAME: 'Connection Diagram',
                                LINK: 'https://cdn.example.com/images/openlines-setup.png',
                                PREVIEW: 'https://cdn.example.com/images/openlines-setup-preview.png',
                                WIDTH: 1280,
                                HEIGHT: 720
                            }
                        ]
                    }
                ]
            },
            KEYBOARD: {
                BUTTONS: [
                    {
                        TEXT: 'Open Instructions',
                        LINK: 'https://help.example.com/openlines/setup',
                        DISPLAY: 'LINE',
                        BG_COLOR_TOKEN: 'primary'
                    }
                ]
            },
            URL_PREVIEW: 'N'
        },
        function(result)
        {
            if (result.error())
            {
                console.error(result.error());
            }
            else
            {
                console.log(result.data());
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'imopenlines.network.message.add',
        [
            'CODE' => 'ab515f5d85a8b844d484f6ea75a2e494',
            'USER_ID' => 2,
            'MESSAGE' => 'We have prepared materials for connecting open channels',
            'ATTACH' => [
                'ID' => 1,
                'COLOR_TOKEN' => 'primary',
                'BLOCKS' => [
                    [
                        'MESSAGE' => 'We have sent a checklist and connection diagram in the attachment'
                    ],
                    [
                        'FILE' => [
                            [
                                'NAME' => 'checklist-openlines.pdf',
                                'LINK' => 'https://cdn.example.com/docs/checklist-openlines.pdf',
                                'SIZE' => 428736
                            ]
                        ]
                    ],
                    [
                        'IMAGE' => [
                            [
                                'NAME' => 'Connection Diagram',
                                'LINK' => 'https://cdn.example.com/images/openlines-setup.png',
                                'PREVIEW' => 'https://cdn.example.com/images/openlines-setup-preview.png',
                                'WIDTH' => 1280,
                                'HEIGHT' => 720
                            ]
                        ]
                    ]
                ]
            ],
            'KEYBOARD' => [
                'BUTTONS' => [
                    [
                        'TEXT' => 'Open Instructions',
                        'LINK' => 'https://help.example.com/openlines/setup',
                        'DISPLAY' => 'LINE',
                        'BG_COLOR_TOKEN' => 'primary'
                    ]
                ]
            ],
            'URL_PREVIEW' => 'N',
        ]
    );

    print_r($result);
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": true,
    "time": {
        "start": 1773740787,
        "finish": 1773740787.828814,
        "duration": 0.8288140296936035,
        "processing": 0,
        "date_start": "2026-03-17T12:46:27+03:00",
        "date_finish": "2026-03-17T12:46:27+03:00",
        "operating_reset_at": 1773741387,
        "operating": 0.17312097549438477
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../data-types.md) | Returns `true` if the message was sent ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "CODE",
    "error_description": "You entered an invalid code"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `403` | `WRONG_AUTH_TYPE` | Access for this method not allowed by session authorization | Method called with session authorization ||
|| `400` | `CODE` | You entered an invalid code | Invalid code in the `CODE` parameter, expected a string of 32 characters ||
|| `400` | `IMBOT_ERROR` | Module IMBOT is not installed | IMBOT module is not installed ||
|| `400` | `NOT_FOUND` | Line not found | Open channel not found ||
|| `400` | `USER_ID_EMPTY` | User ID can't be empty | User identifier not specified ||
|| `400` | `USER_MESSAGE_LIMIT` | You can't send more than one message per week to each user | Message limit exceeded for a specific user ||
|| `400` | `MESSAGE_EMPTY` | Message can't be empty | Message text not specified ||
|| `400` | `ATTACH_OVERSIZE` | You have exceeded the maximum allowable size of attach | Maximum allowable attachment size exceeded 30 KB ||
|| `400` | `ATTACH_ERROR` | Incorrect attach params | Attachment object failed validation ||
|| `400` | `KEYBOARD_ERROR` | Incorrect keyboard params | Keyboard object failed validation ||
|| `400` | `WRONG_REQUEST` | Message isn't added | Message not sent ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./imopenlines-network-join.md)
- [{#T}](../../chats/messages/formatting.md)
- [{#T}](../../chats/messages/attachments.md)
- [{#T}](../../chats/messages/keyboards.md)