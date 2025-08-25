# Event on Task Addition OnTaskAdd

> Scope: [`task`](../../scopes/permissions.md)
>
> Who can subscribe: any user

The event is triggered after a task is created. The following data is passed to the handler:

## What the handler receives

Data is sent as a POST request {.b24-info}

```json
array(
    'event' => 'ONTASKADD',
    'data' => array(
        'FIELDS_BEFORE' => 'undefined',
        'FIELDS_AFTER' => array('ID' => 123),
        'IS_ACCESSIBLE_BEFORE' => 'N',
        'IS_ACCESSIBLE_AFTER' => 'undefined',
    ),
    'ts' => '1466439714',
    'auth' => array(
        'access_token' => 's6p6eclrvim6da22ft9ch94ekreb52lv',
        'expires_in' => '3600',
        'scope' => 'crm',
        'domain' => 'some-domain.bitrix24.com',
        'server_endpoint' => 'https://oauth.bitrix.info/rest/',
        'status' => 'F',
        'client_endpoint' => 'https://some-domain.bitrix24.com/rest/',
        'member_id' => 'a223c6b3710f85df22e9377d6c4f7553',
        'refresh_token' => '4s386p3q0tr8dy89xvmt96234v3dljg8',
        'application_token' => '51856fefc120afa4b628cc82d3935cce',
        ),
)
```

{% include notitle [Footnote on parameters](../../../_includes/required.md) %}

#|
|| **Parameter**
`type` | **Description** ||
|| **event***
[`string`](../../data-types.md) | Symbolic event code, in this case `OnTaskAdd`||
|| **data***
[`array`](../../data-types.md) | Array with data of the added task ||
|| **ts***
[`timestamp`](../../data-types.md) | Date and time of the event sent from the [event queue](../../events/index.md) ||
|| **auth***
[`array`](../../data-types.md) | Authorization parameters and data about the account where the event occurred ||
|#

### Parameter data[]

{% include notitle [Footnote on parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **FIELDS_BEFORE***
[`undefined`\|`object`](../../data-types.md) | Fields of the task before the event (detailed description provided [below](#fields_before)). If there are no available task fields, this field will contain the value `undefined` ||
|| **FIELDS_AFTER***
[`undefined`\|`object`](../../data-types.md) | Fields of the task after the event (detailed description provided [below](#fields_after)). If there are no available task fields, this field will contain the value `undefined` ||
|| **IS_ACCESSIBLE_BEFORE***
[`string`](../../data-types.md) | Whether the task was readable before the event (detailed description provided [below](#is_accessible_before)) ||
|| **IS_ACCESSIBLE_AFTER***
[`string`](../../data-types.md) | Whether the task became readable after the event (detailed description provided [below](#is_accessible_after)) ||
|#

### Field FIELDS_BEFORE {#fields_before}

{% include notitle [Footnote on parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **ID***
[`integer`](../../data-types.md) | Identifier of the created task ||
|#

### Field FIELDS_AFTER {#fields_after}

{% include notitle [Footnote on parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **ID***
[`integer`](../../data-types.md) | Identifier of the created task ||
|#

### Field IS_ACCESSIBLE_BEFORE {#is_accessible_before}

{% include notitle [Footnote on parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **IS_ACCESSIBLE_BEFORE***
[`string`](../../data-types.md) | Possible values:
- `Y` (Yes) — yes
- `N` (No) — no
- `undefined` — not defined or check not performed ||
|#

### Field IS_ACCESSIBLE_AFTER {#is_accessible_after}

{% include notitle [Footnote on parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **IS_ACCESSIBLE_AFTER***
[`string`](../../data-types.md) | Possible values:
- `Y` (Yes) — yes
- `N` (No) — no
- `undefined` — not defined or check not performed ||
|#

## Code Examples

{% include [Footnote on examples](../../../_includes/examples.md) %}

{% list tabs %}

- JS


    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'event.bind',
    		{
    			"event": "onTaskAdd",
    			"handler": "https://example.com/handler.php"
    		}
    	);
    	
    	const result = response.getData().result;
    	console.info(result);
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
                'event.bind',
                [
                    'event'   => 'onTaskAdd',
                    'handler' => 'https://example.com/handler.php',
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
        echo 'Error binding event: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'event.bind',
        {
            "event": "onTaskAdd",
            "handler": "https://example.com/handler.php"
        },
        function(result) {
            if (result.error()) {
                console.error(result.error());
            } else {
                console.info(result.data());
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'event.bind',
        [
            'event' => 'onTaskAdd',
            'handler' => 'https://example.com/handler.php'
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./on-task-update.md)
- [{#T}](./on-task-delete.md)