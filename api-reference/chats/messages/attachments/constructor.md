# Constructor, Examples

{% note warning "We are still updating this page" %}

Some data may be missing — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for writing standards

{% endnote %}

{% endif %}

The **Attachment** object is a constructor; you can "assemble" it as needed using the available blocks. The order of adding blocks matters.

## "Bug Tracker"

This example demonstrates how to use various types of attachments to create an informative and structured message in a bot that simulates a bug tracking system (bug tracker).

{% list tabs %}

- JS


    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'imbot.message.add',
    		{
    			DIALOG_ID: 'chat20921',
    			MESSAGE: 'Message from bot',
    			ATTACH: [
    				{
    					USER: {
    						NAME: "Mantis Notifications",
    						AVATAR: "http://files.shelenkov.com/bitrix/images/mantis2.jpg",
    						LINK: "http://shelenkov.com/",
    					}
    				},
    				{
    					LINK: {
    						NAME: "Open Mantis from external network",
    						LINK: "http://shelenkov.com/",
    					}
    				},
    				{
    					DELIMITER: {
    						SIZE: 200,
    						COLOR: "#c6c6c6"
    					}
    				},
    				{
    					GRID: [
    						{
    							NAME: "Project",
    							VALUE: "BUGS",
    							DISPLAY: "LINE",
    							WIDTH: 100
    						},
    						{
    							NAME: "Category",
    							VALUE: "im",
    							DISPLAY: "LINE",
    							WIDTH: 100
    						},
    						{
    							NAME: "Summary",
    							VALUE: "It is necessary to implement the ability to add structured entities to messages and notifications in the messenger.",
    							DISPLAY: "BLOCK"
    						},
    					]
    				},
    				{
    					DELIMITER: {
    						SIZE: 200,
    						COLOR: "#c6c6c6"
    					}
    				},
    				{
    					GRID: [
    						{
    							NAME: "New Request",
    							VALUE: "",
    							DISPLAY: "ROW",
    							WIDTH: 100
    						},
    						{
    							NAME: "Assigned To",
    							VALUE: "Evgeny Shelenkov",
    							DISPLAY: "ROW",
    							WIDTH: 100
    						},
    						{
    							NAME: "Deadline",
    							VALUE: "11/04/2015 17:50:43",
    							DISPLAY: "ROW",
    							WIDTH: 100
    						},
    					]
    				},
    			]
    		}
    	);
    	
    	const result = response.getData().result;
    	console.log(result);
    }
    catch(error)
    {
    	console.error(error.ex);
    }
    ```

- PHP


    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'imbot.message.add',
                [
                    'DIALOG_ID' => 'chat20921',
                    'MESSAGE' => 'Message from bot',
                    'ATTACH' => [
                        [
                            'USER' => [
                                'NAME' => "Mantis Notifications",
                                'AVATAR' => "http://files.shelenkov.com/bitrix/images/mantis2.jpg",
                                'LINK' => "http://shelenkov.com/",
                            ]
                        ],
                        [
                            'LINK' => [
                                'NAME' => "Open Mantis from external network",
                                'LINK' => "http://shelenkov.com/",
                            ]
                        ],
                        [
                            'DELIMITER' => [
                                'SIZE' => 200,
                                'COLOR' => "#c6c6c6"
                            ]
                        ],
                        [
                            'GRID' => [
                                [
                                    'NAME' => "Project",
                                    'VALUE' => "BUGS",
                                    'DISPLAY' => "LINE",
                                    'WIDTH' => 100
                                ],
                                [
                                    'NAME' => "Category",
                                    'VALUE' => "im",
                                    'DISPLAY' => "LINE",
                                    'WIDTH' => 100
                                ],
                                [
                                    'NAME' => "Summary",
                                    'VALUE' => "It is necessary to implement the ability to add structured entities to messages and notifications in the messenger.",
                                    'DISPLAY' => "BLOCK"
                                ],
                            ]
                        ],
                        [
                            'DELIMITER' => [
                                'SIZE' => 200,
                                'COLOR' => "#c6c6c6"
                            ]
                        ],
                        [
                            'GRID' => [
                                [
                                    'NAME' => "New Request",
                                    'VALUE' => "",
                                    'DISPLAY' => "ROW",
                                    'WIDTH' => 100
                                ],
                                [
                                    'NAME' => "Assigned To",
                                    'VALUE' => "Evgeny Shelenkov",
                                    'DISPLAY' => "ROW",
                                    'WIDTH' => 100
                                ],
                                [
                                    'NAME' => "Deadline",
                                    'VALUE' => "11/04/2015 17:50:43",
                                    'DISPLAY' => "ROW",
                                    'WIDTH' => 100
                                ],
                            ]
                        ],
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
        'imbot.message.add',
        {
            DIALOG_ID: 'chat20921',
            MESSAGE: 'Message from bot',
            ATTACH: [
                {
                    USER: {
                        NAME: "Mantis Notifications",
                        AVATAR: "http://files.shelenkov.com/bitrix/images/mantis2.jpg",
                        LINK: "http://shelenkov.com/",
                    }
                },
                {
                    LINK: {
                        NAME: "Open Mantis from external network",
                        LINK: "http://shelenkov.com/",
                    }
                },
                {
                    DELIMITER: {
                        SIZE: 200,
                        COLOR: "#c6c6c6"
                    }
                },
                {
                    GRID: [
                        {
                            NAME: "Project",
                            VALUE: "BUGS",
                            DISPLAY: "LINE",
                            WIDTH: 100
                        },
                        {
                            NAME: "Category",
                            VALUE: "im",
                            DISPLAY: "LINE",
                            WIDTH: 100
                        },
                        {
                            NAME: "Summary",
                            VALUE: "It is necessary to implement the ability to add structured entities to messages and notifications in the messenger.",
                            DISPLAY: "BLOCK"
                        },
                    ]
                },
                {
                    DELIMITER: {
                        SIZE: 200,
                        COLOR: "#c6c6c6"
                    }
                },
                {
                    GRID: [
                        {
                            NAME: "New Request",
                            VALUE: "",
                            DISPLAY: "ROW",
                            WIDTH: 100
                        },
                        {
                            NAME: "Assigned To",
                            VALUE: "Evgeny Shelenkov",
                            DISPLAY: "ROW",
                            WIDTH: 100
                        },
                        {
                            NAME: "Deadline",
                            VALUE: "11/04/2015 17:50:43",
                            DISPLAY: "ROW",
                            WIDTH: 100
                        },
                    ]
                },
            ]
        }, function(result){
            if(result.error())
            {
                console.error(result.error().ex);
            }
            else
            {
                console.log(result.data());
            }
        }
    );
    ```

- PHP CRest

    {% include [Explanation about restCommand](../../_includes/rest-command.md) %}

    ```php
    restCommand(
        'imbot.message.add',
        Array(
            "DIALOG_ID" => $_REQUEST['data']['PARAMS']['DIALOG_ID'],
            "MESSAGE" => "You have a new notification",
            "ATTACH" => Array(
                Array(
                    "USER" => Array(
                        "NAME" => "Mantis Notifications",
                        "AVATAR" => "http://files.shelenkov.com/bitrix/images/mantis2.jpg",
                        "LINK" => "http://shelenkov.com/",
                    )
                ),
                Array(
                    "LINK" => Array(
                        "NAME" => "Open Mantis from external network",
                        "LINK" => "http://shelenkov.com/",
                    )
                ),
                Array(
                    "DELIMITER" => Array(
                        'SIZE' => 200,
                        'COLOR' => "#c6c6c6"
                    )
                ),
                Array(
                    "GRID" => Array(
                        Array(
                            "NAME" => "Project",
                            "VALUE" => "BUGS",
                            "DISPLAY" => "LINE",
                            "WIDTH" => 100
                        ),
                        Array(
                            "NAME" => "Category",
                            "VALUE" => "im",
                            "DISPLAY" => "LINE",
                            "WIDTH" => 100
                        ),
                        Array(
                            "NAME" => "Summary",
                            "VALUE" => "It is necessary to implement the ability to add structured entities to messages and notifications in the messenger.",
                            "DISPLAY" => "BLOCK"
                        ),
                    )
                ),
                Array(
                    "DELIMITER" => Array(
                        'SIZE' => 200,
                        'COLOR' => "#c6c6c6"
                    )
                ),
                Array(
                    "GRID" => Array(
                        Array(
                            "NAME" => "New Request",
                            "VALUE" => "",
                            "DISPLAY" => "ROW",
                            "WIDTH" => 100
                        ),
                        Array(
                            "NAME" => "Assigned To",
                            "VALUE" => "Evgeny Shelenkov",
                            "DISPLAY" => "ROW",
                            "WIDTH" => 100
                        ),
                        Array(
                            "NAME" => "Deadline",
                            "VALUE" => "11/04/2015 17:50:43",
                            "DISPLAY" => "ROW",
                            "WIDTH" => 100
                        ),
                    )
                ),
            )
        ),
        $_REQUEST["auth"]
    );
    ```

{% endlist %}

{% include [Footnote about examples](../../../../_includes/examples.md) %}

## "Information Leaflet"

This example shows how to create an informational message using various types of attachments, including text and images.

{% list tabs %}

- JS


    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'imbot.message.add',
    		{
    			DIALOG_ID: 'chat20921',
    			MESSAGE: 'You have a new notification',
    			ATTACH: {
    				ID: 1,
    				COLOR: "#29619b",
    				BLOCKS: [
    					{MESSAGE: "Colleagues, the update im 16.0.0 has been checked and is ready for export. A tag needs to be set. We no longer include anything in the update."},
    					{IMAGE: {LINK: "http://files.shelenkov.com/bitrix/images/win.jpg"}}
    				]
    			}
    		}
    	);
    	
    	const result = response.getData().result;
    	console.log(result);
    }
    catch( error )
    {
    	console.error(error.ex);
    }
    ```

- PHP


    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'imbot.message.add',
                [
                    'DIALOG_ID' => 'chat20921',
                    'MESSAGE'   => 'You have a new notification',
                    'ATTACH'    => [
                        'ID'     => 1,
                        'COLOR'  => "#29619b",
                        'BLOCKS' => [
                            ['MESSAGE' => "Colleagues, the update im 16.0.0 has been checked and is ready for export. A tag needs to be set. We no longer include anything in the update."],
                            ['IMAGE'   => ['LINK' => "http://files.shelenkov.com/bitrix/images/win.jpg"]],
                        ],
                    ],
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
        'imbot.message.add',
        {
            DIALOG_ID: 'chat20921',
            MESSAGE: 'You have a new notification',
            ATTACH: {
                ID: 1,
                COLOR: "#29619b",
                BLOCKS: [
                    {MESSAGE: "Colleagues, the update im 16.0.0 has been checked and is ready for export. A tag needs to be set. We no longer include anything in the update."},
                    {IMAGE: {LINK: "http://files.shelenkov.com/bitrix/images/win.jpg"}}
                ]
            }
        }, function(result){
            if(result.error())
            {
                console.error(result.error().ex);
            }
            else
            {
                console.log(result.data());
            }
        }
    );
    ```

- PHP CRest

    {% include [Explanation about restCommand](../../_includes/rest-command.md) %}

    ```php
    restCommand(
        'imbot.message.add',
        Array(
            "DIALOG_ID" => $_REQUEST['data']['PARAMS']['DIALOG_ID'],
            "MESSAGE" => "You have a new notification",
            "ATTACH" => Array(
                Array(
                    "MESSAGE" => "Colleagues, the update im 16.0.0 has been checked and is ready for export. A tag needs to be set. We no longer include anything in the update."
                ),
                Array(
                    "IMAGE" => Array(
                        "LINK" => "http://files.shelenkov.com/bitrix/images/win.jpg",
                    )
                ),
            )
        ),
        $_REQUEST["auth"]
    );
    ```

{% endlist %}

{% include [Footnote about examples](../../../../_includes/examples.md) %}