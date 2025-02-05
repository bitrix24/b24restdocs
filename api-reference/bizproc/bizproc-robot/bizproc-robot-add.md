# Register a New Automation Rule bizproc.robot.add

> Scope: [`bizproc`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

This method registers a new Automation rule.

It only works in the context of the [application](../../app-installation/index.md).

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description**||
|| **CODE***
[`string`](../../data-types.md) | Internal identifier of the Automation rule. It must be unique within the application.

Allowed characters are `a-z`, `A-Z`, `0-9`, dot, hyphen, and underscore `_` ||
|| **HANDLER***
[`string`](../../data-types.md) | URL to which the Automation rule will send data via the Bitrix24 queue server.

The link must have the same domain as the one where the application is installed  ||
|| **AUTH_USER_ID**
[`integer`](../../data-types.md) | Identifier of the user whose token will be passed to the application ||
|| **USE_SUBSCRIPTION**
[`boolean`](../../data-types.md) | Should the Automation rule wait for a response from the application? Possible values:
- `Y` — yes
- `N` — no

||
|| **NAME***
[`string` \| `object`](../../data-types.md) | Name of the Automation rule.

It can be a string or an associative array of localized strings like:

```js
'NAME': {
    'de': 'Automatisierungsregel Name',
    'en': 'Automation rule name',
    ...
},
```

 ||
|| **DESCRIPTION**
[`string` \| `object`](../../data-types.md) | Description of the Automation rule.

It can be a string or an associative array of localized strings like:

```js
'DESCRIPTION': {
    'de': 'Beschreibung der Automatisierungsregel',
    'en': 'Automation rule description',
    ...
},
```
 ||
|| **PROPERTIES**
[`object`](../../data-types.md) | Object with parameters of the Automation rule. Contains objects, each describing a [parameter of the Automation rule](#property).

The system name of the parameter must start with a letter and can contain characters `a-z`, `A-Z`, `0-9`, and underscore `_` ||
|| **RETURN_PROPERTIES**
[`object`](../../data-types.md) | Object with additional results of the Automation rule. Contains objects, each describing a [parameter of the Automation rule](#property).

This parameter controls the ability of the Automation rule to wait for a response from the application and work with the data that will [come in the response](./bizproc-event-send.md).

The system name of the parameter must start with a letter and can contain characters `a-z`, `A-Z`, `0-9`, and underscore `_`
||
|| **DOCUMENT_TYPE**
[`array`](../../data-types.md) | Document type that will determine the data types for the `PROPERTIES` and `RETURN_PROPERTIES` parameters. Consists of three string-type elements: 
- module identifier
- object identifier
- document type

Possible value options:

- CRM Module
    `['crm', 'CCrmDocumentLead', 'LEAD']` — leads
    `['crm', 'CCrmDocumentDeal', 'DEAL']` — deals
    `['crm', 'Bitrix\Crm\Integration\BizProc\Document\Quote', 'QUOTE']` — estimates
    `['crm', 'Bitrix\Crm\Integration\BizProc\Document\SmartInvoice', 'SMART_INVOICE']` — invoices
    `['crm', 'Bitrix\Crm\Integration\BizProc\Document\Dynamic', 'DYNAMIC_XXX']` — SPAs, where XXX is the identifier of the SPA

||
|| **FILTER**
[`object`](../../data-types.md) | Object with rules for restricting the Automation rule by document type and edition.

It can contain keys:
- `INCLUDE` — array of rules where the Automation rule will be displayed
- `EXCLUDE` — array of rules where the Automation rule will be hidden

Each rule in the array can be a string or an array of document types in full or partial form.

To restrict Automation rules by Bitrix24 edition, specify:
- `b24` — for cloud
- `box` — for on-premise

Examples:
1. Exclude the Automation rule for on-premise Bitrix24
    ```js
    'FILTER': {
        EXCLUDE: [ 'box' ]
    }
    ```
2. Display the Automation rule only for deals and leads in CRM
    ```js
    'FILTER': {
        INCLUDE: [
            ['crm', 'CCrmDocumentDeal'],
            ['crm', 'CCrmDocumentLead']
        ]
    }
    ```
||
|| **USE_PLACEMENT**
[`boolean`](../../data-types.md) | Allows opening additional settings for the Automation rule in the application slider. Possible values:
- `Y` — yes
- `N` — no  ||
|#

### Object PROPERTY {#property}

#|
|| **Name**
`type` | **Description**||
|| **Name**
[`string` \| `object`](../../data-types.md) | Name of the parameter ||
|| **Description**
[`string` \| `object`](../../data-types.md) | Description of the parameter ||
|| **Type**
[`string`](../../data-types.md) | Type of the parameter. Basic values: 
  - `bool` — yes or no
  - `date` — date
  - `datetime` — date and time
  - `double` — number
  - `int` — integer 
  - `select` — list
  - `string` — string
  - `text` — text
  - `user` — user  ||
|| **Options**
[`array`](../../data-types.md) | Array of values for the parameter of type list `'TYPE': select'` like:

```js
[
    'value1': 'title1',
    'value2': 'title2',
    'value3': 'title3',
    'value4': 'title4'
]
```
||
|| **Required**
[`boolean`](../../data-types.md) | Parameter requirement. Possible values:
- `Y` — yes
- `N` — no ||
|| **Multiple**
[`boolean`](../../data-types.md) | Parameter multiplicity. Possible values:
- `Y` — yes
- `N` — no ||
|| **Default**
[`any`](../../data-types.md) | Default value of the parameter ||
|#

#### Examples of Objects

```js
// example for select type
'docType': {
    'Name': {
        'de': 'Dokumenttyp',
        'en': 'Document type'
    },
    'Required': 'Y',
    'Multiple': 'N',
    'Default': 'PDF',
    'Type': 'select',
    'Options': {
        'pdf': 'PDF',
        'docx': 'DOCX'
    }
}

// example for bool type
'saveDoc': {
    'Name': {
        'de': 'Dokument speichern',
        'en': 'Save document'
    },
    'Description': {
        'de': 'Einen fortlaufenden Nummer zuweisen',
        'en': 'Assign a sequential number'
    },
    'Type': 'bool',
    'Required': 'Y',
    'Multiple': 'N',
    'Default': 'Y'
}

// example for string type
'Parameters': {
    'Name': {
        'de': 'Vorlagenparameter',
        'en': 'Template\'s parameters'
    },
    'Description': {
        'de': 'ParamID={=ParamValue}',
        'en': 'ParamID={=ParamValue}'
    },
    'Type': 'string',
    'Required': 'N',
    'Multiple': 'Y'
}
```


## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"CODE":"test_robot","HANDLER":"https://your_domain/robot.php","AUTH_USER_ID":1,"USE_SUBSCRIPTION":"Y","NAME":"Send Message","PROPERTIES":{"datetime":{"Name":"When","Type":"datetime"},"text":{"Name":"Text","Type":"text"},"user":{"Name":"To Whom","Type":"user","Default":"Author;"}},"FILTER":{"INCLUDE":[["crm","CCrmDocumentDeal"],["crm","CCrmDocumentLead"]]},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/bizproc.robot.add
    ```

- JS

	```js
    BX24.callMethod(
        'bizproc.robot.add',
        {
            'CODE': 'test_robot',
            'HANDLER': 'https://your_domain/robot.php',
            'AUTH_USER_ID': 1,
            'USE_SUBSCRIPTION': 'Y',
            'NAME': 'Send Message',
            'PROPERTIES': {
                'datetime': {
                    'Name': 'When',
                    'Type': 'datetime'
                },
                'text': {
                    'Name': 'Text',
                    'Type': 'text'
                },
                'user': {
                    'Name': 'To Whom',
                    'Type': 'user',
                    'Default': 'Author;'
                }
            },
            'FILTER': {
                INCLUDE: [
                    ['crm', 'CCrmDocumentDeal'],
                    ['crm', 'CCrmDocumentLead']
                ]
            }
        },
        function(result)
        {
            if(result.error())
                alert("Error: " + result.error());
            else
                alert("Success: " + result.data());
        }
    );
	```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'bizproc.robot.add',
        [
            'CODE' => 'test_robot',
            'HANDLER' => 'https://your_domain/robot.php',
            'AUTH_USER_ID' => 1,
            'USE_SUBSCRIPTION' => 'Y',
            'NAME' => 'Send Message',
            'PROPERTIES' => [
                'datetime' => [
                    'Name' => 'When',
                    'Type' => 'datetime'
                },
                'text' => [
                    'Name' => 'Text',
                    'Type' => 'text'
                },
                'user' => [
                    'Name' => 'To Whom',
                    'Type' => 'user',
                    'Default' => 'Author;'
                ]
            ],
            'FILTER' => [
                'INCLUDE' => [
                    ['crm', 'CCrmDocumentDeal'],
                    ['crm', 'CCrmDocumentLead']
                ]
            ]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- PHP (B24PhpSdk)
  
    ```php
    try {
        $result = $serviceBuilder
            ->getBizProcScope()
            ->robot()
            ->add(
                'robot_code', // string $code
                'https://example.com/handler', // string $handlerUrl
                1, // int $b24AuthUserId
                ['en' => 'Robot Name'], // array $localizedRobotName
                true, // bool $isUseSubscription
                [], // array $properties
                false, // bool $isUsePlacement
                [] // array $returnProperties
            );

        if ($result->isSuccess()) {
            print_r($result->getCoreResponse()->getResponseData()->getResult());
        } else {
            print("Failed to add Automation rule.");
        }
    } catch (Throwable $e) {
        print("Error: " . $e->getMessage());
    }
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": true,
    "time": {
        "start": 1738148752.692647,
        "finish": 1738148752.749058,
        "duration": 0.056411027908325195,
        "processing": 0.018677949905395508,
        "date_start": "2025-01-29T14:05:52+01:00",
        "date_finish": "2025-01-29T14:05:52+01:00",
        "operating_reset_at": 1738149352,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../data-types.md) | Returns `true` if the Automation rule was successfully added ||
|| **time**
[`time`](../../data-types.md#time) | Information about the execution time of the request ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "ERROR_ACTIVITY_VALIDATION_FAILURE",
    "error_description": "Empty activity code!"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Error Message** | **Description** ||
|| `ACCESS_DENIED` | Application context required | Application context is required ||
|| `ACCESS_DENIED` | Access denied! | Method was not executed by an administrator ||
|| `ERROR_ACTIVITY_VALIDATION_FAILURE` | Empty data! | Required fields are missing ||
|| `ERROR_ACTIVITY_VALIDATION_FAILURE` | Empty activity code! | Automation rule code is not specified ||
|| `ERROR_ACTIVITY_VALIDATION_FAILURE` | Wrong activity code! | Invalid Automation rule code ||
|| `ERROR_UNSUPPORTED_PROTOCOL` | Unsupported handler protocol | Invalid handler protocol http, https ||
|| `ERROR_WRONG_HANDLER_URL` | Wrong handler URL | Invalid handler URL ||
|| `ERROR_ACTIVITY_VALIDATION_FAILURE` | Empty activity NAME! | Automation rule name is not specified ||
|| `ERROR_ACTIVITY_VALIDATION_FAILURE` | Wrong properties array! | Incorrectly filled `PROPERTIES` or `RETURN_PROPERTIES` parameters ||
|| `ERROR_ACTIVITY_VALIDATION_FAILURE` | Wrong property key <key>! | Invalid property identifier ||
|| `ERROR_ACTIVITY_VALIDATION_FAILURE` | Empty property NAME <key>! | Property name is not specified ||
|| `ERROR_ACTIVITY_VALIDATION_FAILURE` | Wrong activity FILTER! | Invalid filter ||
|| `ERROR_ACTIVITY_VALIDATION_FAILURE` | Wrong activity DOCUMENT_TYPE! | Invalid `DOCUMENT_TYPE` ||
|| `ERROR_ACTIVITY_ALREADY_INSTALLED` | Activity or Automation rule already installed! | An Automation rule with this code is already installed ||
|| `ERROR_ACTIVITY_ADD_FAILURE` | Activity or Automation rule already added! | Automation rule has already been added ||
|| `ERROR_ACTIVITY_ADD_FAILURE` | Activity save error! | Failed to save the Automation rule, system error ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./index.md)
- [{#T}](./bizproc-robot-update.md)
- [{#T}](./bizproc-robot-list.md)
- [{#T}](./bizproc-robot-delete.md)
- [{#T}](./bizproc-event-send.md)