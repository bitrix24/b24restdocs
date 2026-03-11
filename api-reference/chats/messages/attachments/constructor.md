# Attachment Constructor ATTACH

This page contains practical examples of assembling `ATTACH` from various types of blocks. The final attachment depends on the set and order of the blocks.

## Example 1. "Bug Tracker" Card

A system message with a task card, a link, and tables with parameters.

{% include [Examples Note](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"DIALOG_ID":"chat20921","MESSAGE":"Task Card","ATTACH":[{"USER":{"NAME":"Mantis Notifications","AVATAR":"https://files.shelenkov.com/bitrix/images/mantis2.jpg","LINK":"https://shelenkov.com/"}},{"LINK":{"NAME":"Open Mantis from external network","LINK":"https://shelenkov.com/"}},{"DELIMITER":{"SIZE":200,"COLOR":"#c6c6c6"}},{"GRID":[{"NAME":"Project","VALUE":"BUGS","DISPLAY":"LINE","WIDTH":100},{"NAME":"Category","VALUE":"im","DISPLAY":"LINE","WIDTH":100},{"NAME":"Summary","VALUE":"It is necessary to implement the ability to add structured entities to messages and notifications in the messenger.","DISPLAY":"BLOCK"}]},{"DELIMITER":{"SIZE":200,"COLOR":"#c6c6c6"}},{"GRID":[{"NAME":"New Request","VALUE":"","DISPLAY":"ROW","WIDTH":100},{"NAME":"Assigned To","VALUE":"Evgeny Shelenkov","DISPLAY":"ROW","WIDTH":100},{"NAME":"Deadline","VALUE":"11/04/2015 17:50:43","DISPLAY":"ROW","WIDTH":100}]}]}' \
      https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/im.message.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"DIALOG_ID":"chat20921","MESSAGE":"Task Card","ATTACH":[{"USER":{"NAME":"Mantis Notifications","AVATAR":"https://files.shelenkov.com/bitrix/images/mantis2.jpg","LINK":"https://shelenkov.com/"}},{"LINK":{"NAME":"Open Mantis from external network","LINK":"https://shelenkov.com/"}},{"DELIMITER":{"SIZE":200,"COLOR":"#c6c6c6"}},{"GRID":[{"NAME":"Project","VALUE":"BUGS","DISPLAY":"LINE","WIDTH":100},{"NAME":"Category","VALUE":"im","DISPLAY":"LINE","WIDTH":100},{"NAME":"Summary","VALUE":"It is necessary to implement the ability to add structured entities to messages and notifications in the messenger.","DISPLAY":"BLOCK"}]},{"DELIMITER":{"SIZE":200,"COLOR":"#c6c6c6"}},{"GRID":[{"NAME":"New Request","VALUE":"","DISPLAY":"ROW","WIDTH":100},{"NAME":"Assigned To","VALUE":"Evgeny Shelenkov","DISPLAY":"ROW","WIDTH":100},{"NAME":"Deadline","VALUE":"11/04/2015 17:50:43","DISPLAY":"ROW","WIDTH":100}]}],"auth":"**put_access_token_here**"}' \
      https://**put_your_bitrix24_address**/rest/im.message.add
    ```

- JS

    ```js
    try {
        const response = await $b24.callMethod(
            'im.message.add',
            {
                DIALOG_ID: 'chat20921',
                MESSAGE: 'Task Card',
                ATTACH: [
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
                                VALUE: 'Evgeny Shelenkov',
                                DISPLAY: 'ROW',
                                WIDTH: 100
                            },
                            {
                                NAME: 'Deadline',
                                VALUE: '11/04/2015 17:50:43',
                                DISPLAY: 'ROW',
                                WIDTH: 100
                            }
                        ]
                    }
                ]
            }
        );

        const result = response.getData().result;
        console.log(result);
    } catch (error) {
        console.error(error);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'im.message.add',
                [
                    'DIALOG_ID' => 'chat20921',
                    'MESSAGE' => 'Task Card',
                    'ATTACH' => [
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
                                    'VALUE' => 'Evgeny Shelenkov',
                                    'DISPLAY' => 'ROW',
                                    'WIDTH' => 100
                                ],
                                [
                                    'NAME' => 'Deadline',
                                    'VALUE' => '11/04/2015 17:50:43',
                                    'DISPLAY' => 'ROW',
                                    'WIDTH' => 100
                                ]
                            ]
                        ]
                    ]
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error adding message: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'im.message.add',
        {
            DIALOG_ID: 'chat20921',
            MESSAGE: 'Task Card',
            ATTACH: [
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
                            VALUE: 'Evgeny Shelenkov',
                            DISPLAY: 'ROW',
                            WIDTH: 100
                        },
                        {
                            NAME: 'Deadline',
                            VALUE: '11/04/2015 17:50:43',
                            DISPLAY: 'ROW',
                            WIDTH: 100
                        }
                    ]
                }
            ]
        },
        function(result) {
            if (result.error()) {
                console.error(result.error().ex);
            } else {
                console.log(result.data());
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'im.message.add',
        [
            'DIALOG_ID' => 'chat20921',
            'MESSAGE' => 'Task Card',
            'ATTACH' => [
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
                        },
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
                            'VALUE' => 'Evgeny Shelenkov',
                            'DISPLAY' => 'ROW',
                            'WIDTH' => 100
                        ],
                        [
                            'NAME' => 'Deadline',
                            'VALUE' => '11/04/2015 17:50:43',
                            'DISPLAY' => 'ROW',
                            'WIDTH' => 100
                        ]
                    ]
                ]
            ]
        }
    );

    print_r($result);
    ```

{% endlist %}

## Example 2. Informational Notification

A short informational text and an image as part of a single attachment.

{% include [Examples Note](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"DIALOG_ID":"chat20921","MESSAGE":"You have a new notification","ATTACH":{"ID":1,"COLOR":"#29619b","BLOCKS":[{"MESSAGE":"Colleagues, the im 16.0.0 update has been checked and is ready for release. A tag needs to be set. We no longer include anything in the update."},{"IMAGE":{"LINK":"https://files.shelenkov.com/bitrix/images/win.jpg"}}]}}' \
      https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/im.message.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"DIALOG_ID":"chat20921","MESSAGE":"You have a new notification","ATTACH":{"ID":1,"COLOR":"#29619b","BLOCKS":[{"MESSAGE":"Colleagues, the im 16.0.0 update has been checked and is ready for release. A tag needs to be set. We no longer include anything in the update."},{"IMAGE":{"LINK":"https://files.shelenkov.com/bitrix/images/win.jpg"}}]},"auth":"**put_access_token_here**"}' \
      https://**put_your_bitrix24_address**/rest/im.message.add
    ```

- JS

    ```js
    try {
        const response = await $b24.callMethod(
            'im.message.add',
            {
                DIALOG_ID: 'chat20921',
                MESSAGE: 'You have a new notification',
                ATTACH: {
                    ID: 1,
                    COLOR: '#29619b',
                    BLOCKS: [
                        { MESSAGE: 'Colleagues, the im 16.0.0 update has been checked and is ready for release. A tag needs to be set. We no longer include anything in the update.' },
                        { IMAGE: { LINK: 'https://files.shelenkov.com/bitrix/images/win.jpg' } }
                    ]
                }
            }
        );

        const result = response.getData().result;
        console.log(result);
    } catch (error) {
        console.error(error);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'im.message.add',
                [
                    'DIALOG_ID' => 'chat20921',
                    'MESSAGE' => 'You have a new notification',
                    'ATTACH' => [
                        'ID' => 1,
                        'COLOR' => '#29619b',
                        'BLOCKS' => [
                            ['MESSAGE' => 'Colleagues, the im 16.0.0 update has been checked and is ready for release. A tag needs to be set. We no longer include anything in the update.'],
                            ['IMAGE' => ['LINK' => 'https://files.shelenkov.com/bitrix/images/win.jpg']]
                        ]
                    ]
                }
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error adding message: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'im.message.add',
        {
            DIALOG_ID: 'chat20921',
            MESSAGE: 'You have a new notification',
            ATTACH: {
                ID: 1,
                COLOR: '#29619b',
                BLOCKS: [
                    { MESSAGE: 'Colleagues, the im 16.0.0 update has been checked and is ready for release. A tag needs to be set. We no longer include anything in the update.' },
                    { IMAGE: { LINK: 'https://files.shelenkov.com/bitrix/images/win.jpg' } }
                ]
            }
        },
        function(result) {
            if (result.error()) {
                console.error(result.error().ex);
            } else {
                console.log(result.data());
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'im.message.add',
        [
            'DIALOG_ID' => 'chat20921',
            'MESSAGE' => 'You have a new notification',
            'ATTACH' => [
                'ID' => 1,
                'COLOR' => '#29619b',
                'BLOCKS' => [
                    ['MESSAGE' => 'Colleagues, the im 16.0.0 update has been checked and is ready for release. A tag needs to be set. We no longer include anything in the update.'],
                    ['IMAGE' => ['LINK' => 'https://files.shelenkov.com/bitrix/images/win.jpg']]
                ]
            ]
        }
    );

    print_r($result);
    ```

{% endlist %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./block-collections/index.md)
- [{#T}](../im-message-add.md)