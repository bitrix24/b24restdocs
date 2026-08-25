# Attachment Builder ATTACH

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

This page contains practical examples of assembling `ATTACH` from different types of blocks. The final attachment depends on the set and order of the blocks.

## Example 1. "Bug Tracker" Card

A system message with a task card, a link, and tables with parameters.

{% include [Examples Note](../../../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"botId":456,"botToken":"my_bot_token","dialogId":"chat20921","fields":{"message":"Task Card","attach":[{"USER":{"NAME":"Mantis Notifications","AVATAR":"https://files.shelenkov.com/bitrix/images/mantis2.jpg","LINK":"https://shelenkov.com/"}},{"LINK":{"NAME":"Open Mantis from external network","LINK":"https://shelenkov.com/"}},{"DELIMITER":{"SIZE":200,"COLOR":"#c6c6c6"}},{"GRID":[{"NAME":"Project","VALUE":"BUGS","DISPLAY":"LINE","WIDTH":100},{"NAME":"Category","VALUE":"im","DISPLAY":"LINE","WIDTH":100},{"NAME":"Summary","VALUE":"It is necessary to implement the ability to add structured entities to messages and notifications in the messenger.","DISPLAY":"BLOCK"}]},{"DELIMITER":{"SIZE":200,"COLOR":"#c6c6c6"}},{"GRID":[{"NAME":"New Request","VALUE":"","DISPLAY":"ROW","WIDTH":100},{"NAME":"Assigned To","VALUE":"Eugene Shelenkov","DISPLAY":"ROW","WIDTH":100},{"NAME":"Deadline","VALUE":"11/04/2015 05:50:43 PM","DISPLAY":"ROW","WIDTH":100}]}]}}' \
      https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/imbot.v2.Chat.Message.send
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"botId":456,"dialogId":"chat20921","fields":{"message":"Task Card","attach":[{"USER":{"NAME":"Mantis Notifications","AVATAR":"https://files.shelenkov.com/bitrix/images/mantis2.jpg","LINK":"https://shelenkov.com/"}},{"LINK":{"NAME":"Open Mantis from external network","LINK":"https://shelenkov.com/"}},{"DELIMITER":{"SIZE":200,"COLOR":"#c6c6c6"}},{"GRID":[{"NAME":"Project","VALUE":"BUGS","DISPLAY":"LINE","WIDTH":100},{"NAME":"Category","VALUE":"im","DISPLAY":"LINE","WIDTH":100},{"NAME":"Summary","VALUE":"It is necessary to implement the ability to add structured entities to messages and notifications in the messenger.","DISPLAY":"BLOCK"}]},{"DELIMITER":{"SIZE":200,"COLOR":"#c6c6c6"}},{"GRID":[{"NAME":"New Request","VALUE":"","DISPLAY":"ROW","WIDTH":100},{"NAME":"Assigned To","VALUE":"Eugene Shelenkov","DISPLAY":"ROW","WIDTH":100},{"NAME":"Deadline","VALUE":"11/04/2015 05:50:43 PM","DISPLAY":"ROW","WIDTH":100}]}]},"auth":"**put_access_token_here**"}' \
      https://**put_your_bitrix24_address**/rest/imbot.v2.Chat.Message.send
    ```

- JS

    ```js
    try {
        const response = await $b24.callMethod(
            'imbot.v2.Chat.Message.send',
            {
                botId: 456,
                dialogId: 'chat20921',
                fields: {
                    message: 'Task Card',
                    attach: [
                    {
                        USER: {
                            NAME: 'Mantis Notifications',
                            AVATAR: 'https://files.shelenkov.com/bitrix/images/mantis2.jpg',
                            LINK: 'https://shelenkov.com/'
                        }
                    },
                    {
                        LINK: {
                            NAME: 'Open Mantis from external network',
                            LINK: 'https://shelenkov.com/'
                        }
                    },
                    {
                        DELIMITER: {
                            SIZE: 200,
                            COLOR: '#c6c6c6'
                        }
                    },
                    {
                        GRID: [
                            {
                                NAME: 'Project',
                                VALUE: 'BUGS',
                                DISPLAY: 'LINE',
                                WIDTH: 100
                            },
                            {
                                NAME: 'Category',
                                VALUE: 'im',
                                DISPLAY: 'LINE',
                                WIDTH: 100
                            },
                            {
                                NAME: 'Summary',
                                VALUE: 'It is necessary to implement the ability to add structured entities to messages and notifications in the messenger.',
                                DISPLAY: 'BLOCK'
                            }
                        ]
                    },
                    {
                        DELIMITER: {
                            SIZE: 200,
                            COLOR: '#c6c6c6'
                        }
                    },
                    {
                        GRID: [
                            {
                                NAME: 'New Request',
                                VALUE: '',
                                DISPLAY: 'ROW',
                                WIDTH: 100
                            },
                            {
                                NAME: 'Assigned To',
                                VALUE: 'Eugene Shelenkov',
                                DISPLAY: 'ROW',
                                WIDTH: 100
                            },
                            {
                                NAME: 'Deadline',
                                VALUE: '11/04/2015 05:50:43 PM',
                                DISPLAY: 'ROW',
                                WIDTH: 100
                            }
                        ]
                    }
                    ]
                }
            }
        );

        const result = response.getData().result.id;
        console.log('Created message ID:', result);
    } catch (error) {
        console.error(error);
    }
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.imbot.v2.chat.message.send(
            bot_id=456,
            dialog_id="chat20921",
            fields={
                "message": "Task card",
                "attach": [
                    {
                        "USER": {
                            "NAME": "Mantis notifications",
                            "AVATAR": "https://files.shelenkov.com/bitrix/images/mantis2.jpg",
                            "LINK": "https://shelenkov.com/",
                        },
                    },
                    {
                        "LINK": {
                            "NAME": "Open Mantis from an external network",
                            "LINK": "https://shelenkov.com/",
                        },
                    },
                    {
                        "DELIMITER": {
                            "SIZE": 200,
                            "COLOR": "#c6c6c6",
                        },
                    },
                    {
                        "GRID": [
                            {
                                "NAME": "Project",
                                "VALUE": "BUGS",
                                "DISPLAY": "LINE",
                                "WIDTH": 100,
                            },
                            {
                                "NAME": "Category",
                                "VALUE": "im",
                                "DISPLAY": "LINE",
                                "WIDTH": 100,
                            },
                            {
                                "NAME": "Summary",
                                "VALUE": "We need to implement the ability to add structured entities to messenger messages and notifications.",
                                "DISPLAY": "BLOCK",
                            },
                        ],
                    },
                    {
                        "DELIMITER": {
                            "SIZE": 200,
                            "COLOR": "#c6c6c6",
                        },
                    },
                    {
                        "GRID": [
                            {
                                "NAME": "New ticket",
                                "VALUE": "",
                                "DISPLAY": "ROW",
                                "WIDTH": 100,
                            },
                            {
                                "NAME": "Assigned",
                                "VALUE": "Thomas Schneider",
                                "DISPLAY": "ROW",
                                "WIDTH": 100,
                            },
                            {
                                "NAME": "Deadline",
                                "VALUE": "04.11.2015 17:50:43",
                                "DISPLAY": "ROW",
                                "WIDTH": 100,
                            },
                        ],
                    },
                ],
            },
        ).response
        result = bitrix_response.result["id"]
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
                'imbot.v2.Chat.Message.send',
                [
                    'botId' => 456,
                    'dialogId' => 'chat20921',
                    'fields' => [
                        'message' => 'Task Card',
                        'attach' => [
                        [
                            'USER' => [
                                'NAME' => 'Mantis Notifications',
                                'AVATAR' => 'https://files.shelenkov.com/bitrix/images/mantis2.jpg',
                                'LINK' => 'https://shelenkov.com/'
                            ]
                        ],
                        [
                            'LINK' => [
                                'NAME' => 'Open Mantis from external network',
                                'LINK' => 'https://shelenkov.com/'
                            ]
                        ],
                        [
                            'DELIMITER' => [
                                'SIZE' => 200,
                                'COLOR' => '#c6c6c6'
                            ]
                        ],
                        [
                            'GRID' => [
                                [
                                    'NAME' => 'Project',
                                    'VALUE' => 'BUGS',
                                    'DISPLAY' => 'LINE',
                                    'WIDTH' => 100
                                ],
                                [
                                    'NAME' => 'Category',
                                    'VALUE' => 'im',
                                    'DISPLAY' => 'LINE',
                                    'WIDTH' => 100
                                ],
                                [
                                    'NAME' => 'Summary',
                                    'VALUE' => 'It is necessary to implement the ability to add structured entities to messages and notifications in the messenger.',
                                    'DISPLAY' => 'BLOCK'
                                ]
                            ]
                        ],
                        [
                            'DELIMITER' => [
                                'SIZE' => 200,
                                'COLOR' => '#c6c6c6'
                            ]
                        ],
                        [
                            'GRID' => [
                                [
                                    'NAME' => 'New Request',
                                    'VALUE' => '',
                                    'DISPLAY' => 'ROW',
                                    'WIDTH' => 100
                                ],
                                [
                                    'NAME' => 'Assigned To',
                                    'VALUE' => 'Eugene Shelenkov',
                                    'DISPLAY' => 'ROW',
                                    'WIDTH' => 100
                                ],
                                [
                                    'NAME' => 'Deadline',
                                    'VALUE' => '11/04/2015 05:50:43 PM',
                                    'DISPLAY' => 'ROW',
                                    'WIDTH' => 100
                                ]
                            ]
                        ]
                        ]
                    ]
                ]
            );

        $result = $response->getResponseData()->getResult()['id'];
        echo 'Created message ID: ' . $result;
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error adding message: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'imbot.v2.Chat.Message.send',
        {
            botId: 456,
            dialogId: 'chat20921',
            fields: {
                message: 'Task Card',
                attach: [
                {
                    USER: {
                        NAME: 'Mantis Notifications',
                        AVATAR: 'https://files.shelenkov.com/bitrix/images/mantis2.jpg',
                        LINK: 'https://shelenkov.com/'
                    }
                },
                {
                    LINK: {
                        NAME: 'Open Mantis from external network',
                        LINK: 'https://shelenkov.com/'
                    }
                },
                {
                    DELIMITER: {
                        SIZE: 200,
                        COLOR: '#c6c6c6'
                    }
                },
                {
                    GRID: [
                        {
                            NAME: 'Project',
                            VALUE: 'BUGS',
                            DISPLAY: 'LINE',
                            WIDTH: 100
                        },
                        {
                            NAME: 'Category',
                            VALUE: 'im',
                            DISPLAY: 'LINE',
                            WIDTH: 100
                        },
                        {
                            NAME: 'Summary',
                            VALUE: 'It is necessary to implement the ability to add structured entities to messages and notifications in the messenger.',
                            DISPLAY: 'BLOCK'
                        }
                    ]
                },
                {
                    DELIMITER: {
                        SIZE: 200,
                        COLOR: '#c6c6c6'
                    }
                },
                {
                    GRID: [
                        {
                            NAME: 'New Request',
                            VALUE: '',
                            DISPLAY: 'ROW',
                            WIDTH: 100
                        },
                        {
                            NAME: 'Assigned To',
                            VALUE: 'Eugene Shelenkov',
                            DISPLAY: 'ROW',
                            WIDTH: 100
                        },
                        {
                            NAME: 'Deadline',
                            VALUE: '11/04/2015 05:50:43 PM',
                            DISPLAY: 'ROW',
                            WIDTH: 100
                        }
                    ]
                }
                ]
            }
        },
        function(result) {
            if (result.error()) {
                console.error(result.error().ex);
            } else {
                console.log('Message ID:', result.data().id);
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'imbot.v2.Chat.Message.send',
        [
            'botId' => 456,
            'dialogId' => 'chat20921',
            'fields' => [
                'message' => 'Task Card',
                'attach' => [
                [
                    'USER' => [
                        'NAME' => 'Mantis Notifications',
                        'AVATAR' => 'https://files.shelenkov.com/bitrix/images/mantis2.jpg',
                        'LINK' => 'https://shelenkov.com/'
                    ]
                ],
                [
                    'LINK' => [
                        'NAME' => 'Open Mantis from external network',
                        'LINK' => 'https://shelenkov.com/'
                    ]
                ],
                [
                    'DELIMITER' => [
                        'SIZE' => 200,
                        'COLOR' => '#c6c6c6'
                    ]
                ],
                [
                    'GRID' => [
                        [
                            'NAME' => 'Project',
                            'VALUE' => 'BUGS',
                            'DISPLAY' => 'LINE',
                            'WIDTH' => 100
                        ],
                        [
                            'NAME' => 'Category',
                            'VALUE' => 'im',
                            'DISPLAY' => 'LINE',
                            'WIDTH' => 100
                        ],
                        [
                            'NAME' => 'Summary',
                            'VALUE' => 'It is necessary to implement the ability to add structured entities to messages and notifications in the messenger.',
                            'DISPLAY' => 'BLOCK'
                        ]
                    ]
                ],
                [
                    'DELIMITER' => [
                        'SIZE' => 200,
                        'COLOR' => '#c6c6c6'
                    ]
                ],
                [
                    'GRID' => [
                        [
                            'NAME' => 'New Request',
                            'VALUE' => '',
                            'DISPLAY' => 'ROW',
                            'WIDTH' => 100
                        ],
                        [
                            'NAME' => 'Assigned To',
                            'VALUE' => 'Eugene Shelenkov',
                            'DISPLAY' => 'ROW',
                            'WIDTH' => 100
                        ],
                        [
                            'NAME' => 'Deadline',
                            'VALUE' => '11/04/2015 05:50:43 PM',
                            'DISPLAY' => 'ROW',
                            'WIDTH' => 100
                        ]
                    ]
                ]
                ]
            ]
        ]
    );

    if (!empty($result['error'])) {
        echo 'Error: ' . $result['error_description'];
    } else {
        echo 'Message ID: ' . $result['result']['id'];
    }
    ```

{% endlist %}

## Example 2. Informational Notification

A short informational text and an image as part of a single attachment.

{% include [Examples Note](../../../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"botId":456,"botToken":"my_bot_token","dialogId":"chat20921","fields":{"message":"You have a new notification","attach":{"ID":1,"COLOR":"#29619b","BLOCKS":[{"MESSAGE":"Colleagues, the update im 16.0.0 has been checked and is ready for deployment. A tag needs to be set. We no longer include it in the update."},{"IMAGE":{"LINK":"https://files.shelenkov.com/bitrix/images/win.jpg"}}]}}}' \
      https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/imbot.v2.Chat.Message.send
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"botId":456,"dialogId":"chat20921","fields":{"message":"You have a new notification","attach":{"ID":1,"COLOR":"#29619b","BLOCKS":[{"MESSAGE":"Colleagues, the update im 16.0.0 has been checked and is ready for deployment. A tag needs to be set. We no longer include it in the update."},{"IMAGE":{"LINK":"https://files.shelenkov.com/bitrix/images/win.jpg"}}]}},"auth":"**put_access_token_here**"}' \
      https://**put_your_bitrix24_address**/rest/imbot.v2.Chat.Message.send
    ```

- JS

    ```js
    try {
        const response = await $b24.callMethod(
            'imbot.v2.Chat.Message.send',
            {
                botId: 456,
                dialogId: 'chat20921',
                fields: {
                    message: 'You have a new notification',
                    attach: {
                        ID: 1,
                        COLOR: '#29619b',
                        BLOCKS: [
                            { MESSAGE: 'Colleagues, the update im 16.0.0 has been checked and is ready for deployment. A tag needs to be set. We no longer include it in the update.' },
                            { IMAGE: { LINK: 'https://files.shelenkov.com/bitrix/images/win.jpg' } }
                        ]
                    }
                }
            }
        );

        const result = response.getData().result.id;
        console.log('Created message ID:', result);
    } catch (error) {
        console.error(error);
    }
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.imbot.v2.chat.message.send(
            bot_id=456,
            dialog_id="chat20921",
            fields={
                "message": "You have a new notification",
                "attach": {
                    "ID": 1,
                    "COLOR": "#29619b",
                    "BLOCKS": [
                        {
                            "MESSAGE": "Team, the im 16.0.0 update has been tested and is ready for release. The tag needs to be set. Nothing else goes into this update.",
                        },
                        {
                            "IMAGE": {
                                "LINK": "https://files.shelenkov.com/bitrix/images/win.jpg",
                            },
                        },
                    ],
                },
            },
        ).response
        result = bitrix_response.result["id"]
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
                'imbot.v2.Chat.Message.send',
                [
                    'botId' => 456,
                    'dialogId' => 'chat20921',
                    'fields' => [
                        'message' => 'You have a new notification',
                        'attach' => [
                            'ID' => 1,
                            'COLOR' => '#29619b',
                            'BLOCKS' => [
                                ['MESSAGE' => 'Colleagues, the update im 16.0.0 has been checked and is ready for deployment. A tag needs to be set. We no longer include it in the update.'],
                                ['IMAGE' => ['LINK' => 'https://files.shelenkov.com/bitrix/images/win.jpg']]
                            ]
                        ]
                    ]
                ]
            );

        $result = $response->getResponseData()->getResult()['id'];
        echo 'Created message ID: ' . $result;
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error adding message: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'imbot.v2.Chat.Message.send',
        {
            botId: 456,
            dialogId: 'chat20921',
            fields: {
                message: 'You have a new notification',
                attach: {
                    ID: 1,
                    COLOR: '#29619b',
                    BLOCKS: [
                        { MESSAGE: 'Colleagues, the update im 16.0.0 has been checked and is ready for deployment. A tag needs to be set. We no longer include it in the update.' },
                        { IMAGE: { LINK: 'https://files.shelenkov.com/bitrix/images/win.jpg' } }
                    ]
                }
            }
        },
        function(result) {
            if (result.error()) {
                console.error(result.error().ex);
            } else {
                console.log('Message ID:', result.data().id);
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'imbot.v2.Chat.Message.send',
        [
            'botId' => 456,
            'dialogId' => 'chat20921',
            'fields' => [
                'message' => 'You have a new notification',
                'attach' => [
                    'ID' => 1,
                    'COLOR' => '#29619b',
                    'BLOCKS' => [
                        ['MESSAGE' => 'Colleagues, the update im 16.0.0 has been checked and is ready for deployment. A tag needs to be set. We no longer include it in the update.'],
                        ['IMAGE' => ['LINK' => 'https://files.shelenkov.com/bitrix/images/win.jpg']]
                    ]
                ]
            ]
        ]
    );

    if (!empty($result['error'])) {
        echo 'Error: ' . $result['error_description'];
    } else {
        echo 'Message ID: ' . $result['result']['id'];
    }
    ```

{% endlist %}

## Continue Learning

- [API imbot.v2 Change Log](../../../change-log.md)
- [{#T}](./index.md)
- [{#T}](./block-collections/index.md)
- [{#T}](../chat-message-send.md)