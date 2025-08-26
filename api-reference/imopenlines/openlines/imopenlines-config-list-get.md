# Get the list of open lines imopenlines.config.list.get

{% note warning "We are still updating this page" %}

Some data may be missing — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for writing standards
- missing parameters or fields
- missing examples
- missing success response
- missing error response

{% endnote %}

{% endif %}

> Scope: [`imopenlines`](../../scopes/permissions.md)
>
> Who can execute the method: any user

This method retrieves a list of open lines.

## Method Parameters

#|
|| **Name**
`Type` | **Description** ||
|| **PARAMS**
[`array`](../../data-types.md) | Array of parameters for selection (select, order, filter) (optional). A list of available fields can be found in the description of the method [imopenlines.config.add](./imopenlines-config-add.md) ||
|| **OPTIONS**
[`array`](../../data-types.md) | Array of additional options (optional). Currently includes only the field 'QUEUE' => 'Y'/'N' — queue of responsible employees ||
|#

## Examples

{% include [Note about examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    // example for cURL (Webhook)

- cURL (OAuth)

    // example for cURL (OAuth)

- JS


    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'imopenlines.config.list.get',
    		{
    			PARAMS: {
    				select: [
    					'ID',
    					...
    				],
    				order: {
    					ID: 'ASC',
    					...
    				},
    				filter: {
    					ID: 1,
    					...
    				}
    			},
    			OPTIONS: {
    				QUEUE: 'Y'
    			}
    		}
    	);
    	
    	const result = response.getData().result;
    	if (result.error())
    		alert("Error: " + result.error());
    	else
    		alert("Success: " + result);
    }
    catch( error )
    {
    	console.error('Error:', error);
    }
    ```

- PHP


    ```php
    try {
        $params = [
            'PARAMS' => [
                'select' => [
                    'ID',
                    ...
                ],
                'order' => [
                    'ID' => 'ASC',
                    ...
                ],
                'filter' => [
                    'ID' => 1,
                    ...
                ]
            ],
            'OPTIONS' => [
                'QUEUE' => 'Y'
            ]
        ];
    
        $response = $b24Service
            ->core
            ->call(
                'imopenlines.config.list.get',
                $params
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        if ($result->error()) {
            echo 'Error: ' . $result->error();
        } else {
            echo 'Success: ' . $result->data();
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error calling imopenlines.config.list.get: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    //imopenlines.config.list.get
    function configListGet()
    {
        var params = {
            PARAMS: {
                select: [
                    'ID',
                    ...
                ],
                order: {
                    ID: 'ASC',
                    ...
                },
                filter: {
                    ID: 1,
                    ...
                }
            },
            OPTIONS: {
                QUEUE: 'Y'
            }
        };
        BX24.callMethod(
            'imopenlines.config.list.get',
            params,
            function (result) {
                if (result.error())
                    alert("Error: " + result.error());
                else
                    alert("Success: " + result.data());
            }
        );
    }
    ```

- PHP CRest

    // example for php

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": [
        {
            "ID": "1",
            "ACTIVE": "Y",
            "LINE_NAME": "WhatsApp",
            "CRM": "Y",
            "CRM_CREATE": "lead",
            "CRM_CREATE_SECOND": "0",
            "CRM_CREATE_THIRD": "Y",
            "CRM_FORWARD": "Y",
            "CRM_CHAT_TRACKER": "N",
            "CRM_TRANSFER_CHANGE": "Y",
            "CRM_SOURCE": "1|WZ_WHATSAPP_CASJK2QWBRWQ5ASBQWKEBN4QWBENAL2BA",
            "QUEUE_TIME": "900",
            "NO_ANSWER_TIME": "60",
            "QUEUE_TYPE": "all",
            "CHECK_AVAILABLE": "N",
            "WATCH_TYPING": "Y",
            "WELCOME_BOT_ENABLE": "N",
            "WELCOME_MESSAGE": "N",
            "WELCOME_MESSAGE_TEXT": "Welcome to the Open Line [br]The first available operator will respond to you.",
            "VOTE_MESSAGE": "N",
            "VOTE_TIME_LIMIT": "0",
            "VOTE_BEFORE_FINISH": "Y",
            "VOTE_CLOSING_DELAY": "N",
            "VOTE_MESSAGE_1_TEXT": "Please rate the quality of service.",
            "VOTE_MESSAGE_1_LIKE": "Thank you for your feedback!",
            "VOTE_MESSAGE_1_DISLIKE": "We are sorry that we couldn't assist you, we will strive to improve.",
            "VOTE_MESSAGE_2_TEXT": "Please rate the quality of service.\r\n\r\nSend: 1 - good, 0 - bad",
            "VOTE_MESSAGE_2_LIKE": "Thank you for your feedback!",
            "VOTE_MESSAGE_2_DISLIKE": "We are sorry that we couldn't assist you, we will strive to improve.",
            "AGREEMENT_MESSAGE": "N",
            "AGREEMENT_ID": "0",
            "CATEGORY_ENABLE": "N",
            "CATEGORY_ID": "0",
            "WELCOME_BOT_JOIN": "always",
            "WELCOME_BOT_ID": "0",
            "WELCOME_BOT_TIME": "600",
            "WELCOME_BOT_LEFT": "queue",
            "NO_ANSWER_RULE": "none",
            "NO_ANSWER_FORM_ID": "0",
            "NO_ANSWER_BOT_ID": "0",
            "NO_ANSWER_TEXT": "Unfortunately, we cannot respond to you at this moment, we will definitely get back to you.",
            "WORKTIME_ENABLE": "N",
            "WORKTIME_FROM": "9",
            "WORKTIME_TO": "18",
            "WORKTIME_TIMEZONE": "Europe/Berlin",
            "WORKTIME_HOLIDAYS": [
                ""
            ],
            "WORKTIME_DAYOFF": [
                "SA",
                "SU"
            ],
            "WORKTIME_DAYOFF_RULE": "text",
            "WORKTIME_DAYOFF_FORM_ID": "0",
            "WORKTIME_DAYOFF_BOT_ID": "0",
            "WORKTIME_DAYOFF_TEXT": "Welcome to the Open Line [br]Unfortunately, we cannot respond to you at this moment.[br][br]Please write your question and we will definitely get back to you during working hours.",
            "CLOSE_RULE": "none",
            "CLOSE_FORM_ID": "0",
            "CLOSE_BOT_ID": "0",
            "CLOSE_TEXT": "Thank you for contacting our company.",
            "FULL_CLOSE_TIME": "10",
            "AUTO_CLOSE_RULE": "none",
            "AUTO_CLOSE_FORM_ID": "0",
            "AUTO_CLOSE_BOT_ID": "0",
            "AUTO_CLOSE_TIME": "2678400",
            "AUTO_CLOSE_TEXT": "",
            "AUTO_EXPIRE_TIME": "86400",
            "DATE_CREATE": {},
            "DATE_MODIFY": {},
            "MODIFY_USER_ID": "177",
            "TEMPORARY": "N",
            "XML_ID": null,
            "LANGUAGE_ID": "de",
            "QUICK_ANSWERS_IBLOCK_ID": "53",
            "SESSION_PRIORITY": "0",
            "TYPE_MAX_CHAT": "answered",
            "MAX_CHAT": "0",
            "OPERATOR_DATA": "queue",
            "DEFAULT_OPERATOR_DATA": [],
            "KPI_FIRST_ANSWER_TIME": "0",
            "KPI_FIRST_ANSWER_ALERT": "N",
            "KPI_FIRST_ANSWER_LIST": false,
            "KPI_FIRST_ANSWER_TEXT": "Employee #OPERATOR# exceeded the allowable response time to the client's first message. Dialog №#DIALOG#.",
            "KPI_FURTHER_ANSWER_TIME": "0",
            "KPI_FURTHER_ANSWER_ALERT": "N",
            "KPI_FURTHER_ANSWER_LIST": false,
            "KPI_FURTHER_ANSWER_TEXT": "Employee #OPERATOR# exceeded the allowable response time to the client's message. Dialog №#DIALOG#.",
            "KPI_CHECK_OPERATOR_ACTIVITY": "N",
            "SEND_NOTIFICATION_EMPTY_QUEUE": "N",
            "USE_WELCOME_FORM": "N",
            "WELCOME_FORM_ID": "5",
            "WELCOME_FORM_DELAY": "Y",
            "SEND_WELCOME_EACH_SESSION": "N",
            "CONFIRM_CLOSE": "Y",
            "IGNORE_WELCOME_FORM_RESPONSIBLE": "N"
        },
        // .. 49 more items
    ],
    "next": 50,
    "total": 123456,
    "time": {
        "start": 1730383163.284897,
        "finish": 1730383163.308128,
        "duration": 0.023231029510498047,
        "processing": 0.001950979232788086,
        "date_start": "2024-10-31T16:59:23+02:00",
        "date_finish": "2024-10-31T16:59:23+02:00",
        "operating_reset_at": 1730383763,
        "operating": 0
    }
}
```