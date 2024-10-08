# Widget Handler Installation

> Method Name: **placement.bind**
>
> Scope: [`placement`, `depending on the placement location`](../scopes/permissions.md)
>
> Who can execute the method: administrator

This method adds a handler for the widget placement.

It can be called at any time during the application's operation; however, it is often more convenient to register your widgets during the [application installation](../app-installation/index.md).

It is important to note that while the application installation is not complete, the widgets you register will not be available to regular users in the Bitrix24 interface - they will only be visible to users with administrative rights.

It is recommended to familiarize yourself with the principles of [application installation](../app-installation/index.md) in Bitrix24.

## Method Parameters {#params}

{% include [Note on Required Parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **PLACEMENT***
[`string`](../data-types.md) | Identifier for the required widget placement location ||
|| **HANDLER***
[`string`](../data-types.md) | URL of the widget placement handler ||
|| **TITLE**
[`string`](../data-types.md) | Name of the widget in the interface. Depending on the specific placement location, this may be the name of a tab in a form, the name of a menu item, etc. ||
|| **DESCRIPTION**
[`string`](../data-types.md) | Description of the widget in the interface. Not used in practice ||
|| **GROUP_NAME**
[`string`](../data-types.md) | Allows grouping UI elements for multiple handlers of the same type of widget. For example, several dropdown menu items in the [top button of the CRM detail form](./crm/detail-toolbar.md). Supported only by certain types of widgets ||
|| **LANG_ALL**
[`object`](../data-types.md) | Array of parameters `TITLE`, `DESCRIPTION`, and `GROUP_NAME` for specified languages. Users who have selected one of these languages in the Bitrix24 interface will see localized versions of `TITLE`, `DESCRIPTION`, and `GROUP_NAME`: 

```json

    "LANG_ALL": {
        "en": {
            "TITLE": "title",
            "DESCRIPTION": "description",
            "GROUP_NAME": "group"
        },
		"de": {
        	"TITLE": "Titel",
        	"DESCRIPTION": "Beschreibung",
        	"GROUP_NAME": "Gruppe"
    	}
    }

```

||
|| **OPTIONS**
[`object`](../data-types.md) | Additional display parameters for the widget. Specific values depend on the widget placement location. Currently used in widgets for messengers and in the widget [`PAGE_BACKGROUND_WORKER`](./universal/background-worker.md)

||
|| **USER_ID**
[`integer`](../data-types.md) | Identifier of the Bitrix24 user for whom the registered widget will be available. Possible values can be obtained using the [user.get](../user/user-get.md) method.

Currently, this parameter is only supported by the widget [`PAGE_BACKGROUND_WORKER`](./universal/background-worker.md).

If you attempt to register a placement in other widgets, you will receive the error `ERROR_PLACEMENT_USER_MODE: User mode is not available`.

||
|#

## Code Examples

{% include [Note on Examples](../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"PLACEMENT":"PLACEMENT_CODE","HANDLER":"http://myapp.com/handler/?type=1","OPTIONS":{"errorHandlerUrl":"http://myapp.com/error/"},"TITLE":"title","DESCRIPTION":"description","GROUP_NAME":"group","LANG_ALL":{"en":{"TITLE":"title","DESCRIPTION":"description","GROUP_NAME":"group"}}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/placement.bind
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"PLACEMENT":"PLACEMENT_CODE","HANDLER":"http://myapp.com/handler/?type=1","OPTIONS":{"errorHandlerUrl":"http://myapp.com/error/"},"TITLE":"title","DESCRIPTION":"description","GROUP_NAME":"group","LANG_ALL":{"en":{"TITLE":"title","DESCRIPTION":"description","GROUP_NAME":"group"}},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/placement.bind
    ```

- JS

    ```js
    BX24.callMethod(
        "placement.bind",
        { 
            "PLACEMENT": "PLACEMENT_CODE",
            "HANDLER": "http://myapp.com/handler/?type=1",
            "OPTIONS": {
                "errorHandlerUrl": "http://myapp.com/error/"
            },
            "TITLE": "title",
            "DESCRIPTION": "description",
            "GROUP_NAME": "group",
            "LANG_ALL": {
                "en": {
                    "TITLE": "title",
                    "DESCRIPTION": "description",
                    "GROUP_NAME": "group",
                },
				"de": {
        			"TITLE": "Titel",
       			 	"DESCRIPTION": "Beschreibung",
       				"GROUP_NAME": "Gruppe"
    			}
            }
        },
        function(result)
        {
            if(result.error())
                console.error(result.error());
            else
                console.info(result.data());
        }
    );
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'placement.bind',
        [
            'PLACEMENT' => 'PLACEMENT_CODE',
            'HANDLER' => 'http://myapp.com/handler/?type=1',
            'OPTIONS' => [
                'errorHandlerUrl' => 'http://myapp.com/error/'
            ],
            'TITLE' => 'title',
            'DESCRIPTION' => 'description',
            'GROUP_NAME' => 'group',
            'LANG_ALL' => [
                'en' => [
                    'TITLE' => 'title',
                    'DESCRIPTION' => 'description',
                    'GROUP_NAME' => 'group'
                ],
				'de' => [
        			'TITLE' => 'Titel',
       			 	'DESCRIPTION' => 'Beschreibung',
        			'GROUP_NAME' => 'Gruppe'
    			]
            ]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

{% note tip "Typical use-cases and scenarios" %}

- [{#T}](../../tutorials/crm/crm-widgets/widget-as-detail-tab.md)
- [{#T}](../../tutorials/crm/crm-widgets/widget-as-field-in-lead-page.md)

{% endnote %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": true,
    "time": {
        "start": 1712132792.910734,
        "finish": 1712132793.530359,
        "duration": 0.6196250915527344,
        "processing": 0.032338857650756836,
        "date_start": "2024-04-03T10:26:32+02:00",
        "date_finish": "2024-04-03T10:26:33+02:00",
        "operating_reset_at": 1705765533,
        "operating": 3.3076241016387939
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../data-types.md) | Returns the result of adding the widget handler. Possible values:

- `True`, the handler was successfully registered;
- `False`, the handler was not registered
||
|| **time**
[`time`](../data-types.md) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**, **403**, **200**

```json
{
    "error": "ERROR_ARGUMENT",
    "error_description": "Argument 'PLACEMENT' is null or empty",
    "argument": "PLACEMENT"
}
```

{% include notitle [Error Handling](../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Status** ||
|| `ERROR_PLACEMENT_MAX_COUNT` | Attempted to re-register the handler for the `PAGE_BACKGROUND_WORKER` widget | 200 ||
|| `ERROR_ARGUMENT` | Required field value is not specified. The code of the required field is returned in `argument`| 200 ||
|#

{% include [System Errors](../../_includes/system-errors.md) %}

## Widget Handler

Thus, a successful call to the `placement.bind` method allowed you to register the widget handler. It is important that the HANDLER_URL parameter you specified points to a real and accessible URL.

{% note warning "Important" %}

The handler URL must be **accessible** from the external network. Links to localhost, local domains, and similar methods of accessing a local web server are not acceptable. Check the availability of the URL you specified using special services that monitor website accessibility!

{% endnote %}

When accessing your handler, Bitrix24 will send a POST message containing information about the widget context, such as the deal identifier if the widget is embedded in the deal card in CRM, etc.

You can find examples of such data in the descriptions of [specific widget placement locations](./placements.md).

## Continue Learning

- [{#T}](./placements.md)
- [{#T}](./placement-list.md)
- [{#T}](./placement-unbind.md)
- [{#T}](./ui-interaction/index.md)