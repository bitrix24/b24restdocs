# Add a new open channel imopenlines.config.add

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for standard writing
- parameter types are not specified
- parameter requirements are not indicated
- examples are missing
- success response is missing
- error response is missing

{% endnote %}

{% endif %}

> Scope: [`imopenlines`](../../scopes/permissions.md)
>
> Who can execute the method: any user

This method adds a new open channel.

## Method Parameters

#|
|| **Name**
`Type`  | **Description** | **Available from** ||
|| **PARAMS**
[`unknown`](../../data-types.md) | Array of parameters to add (optional). List of fields — below | ||
|#

## List of Fields

#|
|| **Name**
`Type` | **Description** ||
|| **WELCOME_BOT_ENABLE**
[`unknown`](../../data-types.md) | Assign a chatbot as responsible when a client contacts. [Y/N (default)] — this option must be Y for the bot to work ||
|| **WELCOME_BOT_JOIN**
[`unknown`](../../data-types.md) | When to connect the chatbot (`first` (default), always) ||
|| **WELCOME_BOT_ID**
[`unknown`](../../data-types.md) | Bot identifier (int, default 0) ||
|| **WELCOME_BOT_TIME**
[`unknown`](../../data-types.md) | After how long to transfer the conversation from the chatbot to the queue (int, 60 by default) ||
|| **WELCOME_BOT_LEFT**
[`unknown`](../../data-types.md) | When to disconnect the chatbot (`queue` (default), close) ||
|| **ACTIVE**
[`unknown`](../../data-types.md) | Line activity [Y/N (default)] ||
|| **LINE_NAME**
[`unknown`](../../data-types.md) | Line name (optional) ||
|| **CRM**
[`unknown`](../../data-types.md) | Check user in CRM [Y/N (default)] ||
|| **CRM_CREATE**
[`unknown`](../../data-types.md) | If the client is not found in CRM (string, default `none`) ||
|| **CRM_FORWARD**
[`unknown`](../../data-types.md) | Forward the request to the responsible employee if the client is identified [Y (default)/N] ||
|| **CRM_SOURCE**
[`unknown`](../../data-types.md) | Source for the new lead (string, default 'create') ||
|| **CRM_TRANSFER_CHANGE**
[`unknown`](../../data-types.md) | Automatically change the responsible person for the lead when manually redirecting the request to another operator [Y (default)/N] ||
|| **QUEUE_TIME**
[`unknown`](../../data-types.md) | Time before the request is passed to the next employee in the queue (int, 60 by default) ||
|| **NO_ANSWER_TIME**
[`unknown`](../../data-types.md) | Time before marking the message as unanswered (int, 60 by default) ||
|| **QUEUE_TYPE**
[`unknown`](../../data-types.md) | Queue type (`evenly` (default), strictly, all) ||
|| **TIMEMAN**
[`unknown`](../../data-types.md) | Do not direct the request to the operator if the workday has not started or a break is set [Y/N (default)] ||
|| **CHECK_ONLINE**
[`unknown`](../../data-types.md) | Check operator availability when distributing requests [Y/N (default)] ||
|| **CHECKING_OFFLINE**
[`unknown`](../../data-types.md) | Constantly check operator availability when distributing requests [Y/N (default)] ||
|| **WELCOME_MESSAGE**
[`unknown`](../../data-types.md) | Send an automatic response to the client's first message [Y (default)/N] ||
|| **WELCOME_MESSAGE_TEXT**
[`unknown`](../../data-types.md) | Text of the automatic response (string, default null) ||
|| **AGREEMENT_MESSAGE**
[`unknown`](../../data-types.md) | Send a warning about the collection of personal data [Y/N (default)] ||
|| **AGREEMENT_ID**
[`unknown`](../../data-types.md) | Agreement ID in the system (int, 0 by default) ||
|| **NO_ANSWER_RULE**
[`unknown`](../../data-types.md) | Action if operators do not respond to the request (`none` (default), text) ||
|| **NO_ANSWER_TEXT**
[`unknown`](../../data-types.md) | Text of the automatic response (string, default `null`) ||
|| **WORKTIME_ENABLE**
[`unknown`](../../data-types.md) | Set working hours for the Open Channel [Y/N (default)] ||
|| **WORKTIME_FROM**
[`unknown`](../../data-types.md) | Working hours "from" (string format '00:00') ||
|| **WORKTIME_TO**
[`unknown`](../../data-types.md) | Working hours "to" (string format '00:00') ||
|| **WORKTIME_TIMEZONE**
[`unknown`](../../data-types.md) | Time zone (format type 'Europe/Riga') ||
|| **WORKTIME_HOLIDAYS**
[`unknown`](../../data-types.md) | List of holidays (string, Example: 1.01,2.01,7.01,23.02,8.03,1.05,9.05,12.06,4.11,12.12) ||
|| **WORKTIME_DAYOFF**
[`unknown`](../../data-types.md) | List of day-off codes (array, example ['MO', 'TU']) ||
|| **WORKTIME_DAYOFF_RULE**
[`unknown`](../../data-types.md) | Handling requests during non-working hours (`none` (default), 'text') ||
|| **WORKTIME_DAYOFF_TEXT**
[`unknown`](../../data-types.md) | Text of the automatic response (during non-working hours) (string, default `null`) ||
|| **CLOSE_RULE**
[`unknown`](../../data-types.md) | Action upon client request completion ('none', `text` (default)) ||
|| **CLOSE_TEXT**
[`unknown`](../../data-types.md) | Text of the automatic response (string, default `null`) ||
|| **FULL_CLOSE_TIME**
[`unknown`](../../data-types.md) | Time until complete closure of the request (from the moment it is closed by the operator) (int, default 10 minutes) ||
|| **AUTO_CLOSE_RULE**
[`unknown`](../../data-types.md) | Action to be performed upon automatic closure (`none` (default), 'text') ||
|| **AUTO_CLOSE_TEXT**
[`unknown`](../../data-types.md) | Text of the automatic response (string, default null) ||
|| **AUTO_CLOSE_TIME**
[`unknown`](../../data-types.md) | Time of last activity for auto-closing the dialogue (int, default 0) ||
|| **VOTE_MESSAGE**
[`unknown`](../../data-types.md) | Send a request to the client for service quality evaluation, char(1), [Y (default)/N] ||
|| **VOTE_CLOSING_DELAY**
[`unknown`](../../data-types.md) | Close the session immediately after the client evaluation, char(1), [Y/N (default)] ||
|| **VOTE_MESSAGE_1_TEXT**
[`unknown`](../../data-types.md) | Text for the evaluation request in online chat and Bitrix24 Network ||
|| **VOTE_MESSAGE_1_LIKE**
[`unknown`](../../data-types.md) | Text for positive evaluation in online chat and Bitrix24 Network ||
|| **VOTE_MESSAGE_1_DISLIKE**
[`unknown`](../../data-types.md) | Text for negative evaluation in online chat and Bitrix24 Network ||
|| **VOTE_MESSAGE_2_TEXT**
[`unknown`](../../data-types.md) | Text for the assessment request in other channels (Viber, Telegram, Facebook and others) ||
|| **VOTE_MESSAGE_2_LIKE**
[`unknown`](../../data-types.md) | Text for positive assessment in other channels (Viber, Telegram, Facebook and others) ||
|| **VOTE_MESSAGE_2_DISLIKE**
[`unknown`](../../data-types.md) | Text for negative assessment in other channels (Viber, Telegram, Facebook and others) ||
|| **QUICK_ANSWERS_IBLOCK_ID**
[`unknown`](../../data-types.md) | Identifier of the quick answers info block (default, 0) ||
|| **LANGUAGE_ID**
[`unknown`](../../data-types.md) | Language preference settings (char(2), default `null`) ||
|| **OPERATOR_DATA**
[`unknown`](../../data-types.md) | Information about operators in the queue (`profile` (default), queue, hide) ||
|| **DEFAULT_OPERATOR_DATA**
[`unknown`](../../data-types.md) | Default operator information. Array, fields:
- NAME — name
- AVATAR — link to avatar
- AVATAR_ID — identifier of the avatar file on the account ||
|| **QUEUE**
[`unknown`](../../data-types.md) | Queue of responsible employees. Array, fields:
- U — array of user ids to add to the queue. Can be passed in the format
    ```js
    `QUEUE: [
        {
            ENTITY_TYPE: "user",
            ENTITY_ID: "1"
        }
    ]
    ```
 ||
|| **QUEUE_OPERATOR_DATA**
[`unknown`](../../data-types.md) | Operator data for display in chat. Array, fields:
- U — array of users with data of the type "User ID" => array of data:
  - NAME — name
  - USER_WORK_POSITION — position
  - AVATAR — link to avatar
  - AVATAR_ID — identifier of the avatar file on the account ||
|#

## Examples

{% include [Example notes](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    // example for cURL (Webhook)

- cURL (OAuth)

    // example for cURL (OAuth)

- JS


    ```js
    try
    {
    	const params = {
    		PARAMS: {
    			LINE_NAME: 'New line name',
    			...
    		}
    	};
    	
    	const response = await $b24.callMethod(
    		'imopenlines.config.add',
    		params
    	);
    	
    	const result = response.getData().result;
    	if (result.error())
    		alert("Error: " + result.error());
    	else
    		alert("Successfully: " + result);
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
                'LINE_NAME' => 'New line name',
                // Other parameters
            ],
        ];
    
        $response = $b24Service
            ->core
            ->call(
                'imopenlines.config.add',
                $params
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        if ($result->error()) {
            echo 'Error: ' . $result->error();
        } else {
            echo 'Successfully: ' . $result->data();
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error adding configuration: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    //imopenlines.config.add
    function configAdd()
    {
        var params = {
            PARAMS: {
                LINE_NAME: 'New line name',
                ...
            }
        };
        BX24.callMethod(
            'imopenlines.config.add',
            params,
            function (result) {
                if (result.error())
                    alert("Error: " + result.error());
                else
                    alert("Successfully: " + result.data());
            }
        );
    }
    ```

- PHP CRest

    // example for php

{% endlist %}