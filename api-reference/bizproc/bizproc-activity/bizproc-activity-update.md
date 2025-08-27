# Update Action bizproc.activity.update

> Scope: [`bizproc`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

This method updates a business process action added by the application.

It works only in the context of the [application](../../app-installation/index.md).

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description**||
|| **CODE***
[`string`](../../data-types.md) | Internal identifier of the action ||
|| **FIELDS***
[`object`](../../data-types.md) | Object with [fields](#parametr-fields) of the business process action ||
|#

### Parameter FIELDS {#parametr-fields}

#|
|| **Name**
`type` | **Description**||
|| **HANDLER***
[`string`](../../data-types.md) | URL to which the action will send data via the Bitrix24 queue server.

The link must have the same domain where the application is installed ||
|| **AUTH_USER_ID**
[`integer`](../../data-types.md) | Identifier of the user whose token will be passed to the application ||
|| **USE_SUBSCRIPTION**
[`boolean`](../../data-types.md) | Should the action wait for a response from the application. Possible values:
- `Y` — yes
- `N` — no
||
|| **NAME***
[`string` \| `object`](../../data-types.md) | Name of the action.

Can be a string or an associative array of localized strings like:

```js
'NAME': {
    'de': 'Aktionsname',
    'en': 'action name',
    ...
},
```

||
|| **DESCRIPTION**
[`string` \| `object`](../../data-types.md) | Description of the action.

Can be a string or an associative array of localized strings like:

```js
'DESCRIPTION': {
    'de': 'Aktionsbeschreibung',
    'en': 'action description',
    ...
},
```
 ||
|| **PROPERTIES**
[`object`](../../data-types.md) | Object with parameters of the action. Contains objects, each describing a [parameter of the action](#property).

The system name of the parameter must start with a letter and can contain characters `a-z`, `A-Z`, `0-9`, and underscore `_` ||
|| **RETURN_PROPERTIES**
[`object`](../../data-types.md) | Object with additional results of the action. Contains objects, each describing a [parameter of the action](#property).

This parameter controls the ability of the action to wait for a response from the application and work with the data that will [come in the response](../bizproc-robot/bizproc-event-send.md).

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
    `['crm', 'CCrmDocumentContact', 'CONTACT']` — contacts
    `['crm', 'CCrmDocumentCompany', 'COMPANY']` — companies
    `['crm', 'CCrmDocumentDeal', 'DEAL']` — deals
    `['crm', 'Bitrix\Crm\Integration\BizProc\Document\Quote', 'QUOTE']` — estimates
    `['crm', 'Bitrix\Crm\Integration\BizProc\Document\SmartInvoice', 'SMART_INVOICE']` — invoices
    `['crm', 'Bitrix\Crm\Integration\BizProc\Document\Dynamic', 'DYNAMIC_XXX']` — SPAs, where XXX is the identifier of the SPA

- Lists Module
    `['lists', 'BizprocDocument', 'iblock_XXX']` — processes in the news feed, where XXX is the identifier of the information block
    `['lists', 'Bitrix\Lists\BizprocDocumentLists', 'iblock_XXX']` — lists in groups, where XXX is the identifier of the information block

- Drive Module
    `['disk', 'Bitrix\Disk\BizProcDocument', 'STORAGE_XXX']`, where XXX is the storage identifier

||
|| **FILTER**
[`object`](../../data-types.md) | Object with rules to restrict the action by document type and edition.

May contain keys:
- `INCLUDE` — array of rules where the action will be displayed
- `EXCLUDE` — array of rules where the action will be hidden

Each rule in the array can be a string or an array of document types in full or partial form.

To restrict the action by Bitrix24 edition, specify:
- `b24` — for cloud
- `box` — for on-premise

Examples:
1. Exclude action for on-premise Bitrix24
    ```js
    FILTER: {
        EXCLUDE: [ 'box' ]
    }
    ```
2. Display action only for the Lists module
    ```js
    FILTER: {
        INCLUDE: [
            ['lists']
        ]
    }
    ```
3. Display action only for the Lists module and deals from CRM
    ```js
    FILTER: {
        INCLUDE: [
            ['lists'],
            ['crm', 'CCrmDocumentDeal']
        ]
    }
    ```
||
|| **USE_PLACEMENT**
[`boolean`](../../data-types.md) | Allows opening additional settings of the action in the application slider. Possible values:
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
[`boolean`](../../data-types.md) | Requirement of the parameter. Possible values:
- `Y` — yes
- `N` — no ||
|| **Multiple**
[`boolean`](../../data-types.md) | Multiplicity of the parameter. Possible values:
- `Y` — yes
- `N` — no ||
|| **Default**
[`any`](../../data-types.md) | Default value of the parameter ||
|#

#### Example Objects

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
    -d '{"CODE":"action_test_code","FIELDS":{"AUTH_USER_ID":1,"USE_SUBSCRIPTION":"N","FILTER":{"INCLUDE":[["lists"],["crm","CCrmDocumentDeal"]]}}, "auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/bizproc.activity.update
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'bizproc.activity.update',
    		{
    			'CODE': 'action_test_code',
    			'FIELDS': {
    				'AUTH_USER_ID': 1,
    				'USE_SUBSCRIPTION': 'N',
    				'FILTER': {
    					'INCLUDE': [
    						['lists'],
    						['crm', 'CCrmDocumentDeal']
    					]
    				}
    			},
    		}
    	);
    	
    	const result = response.getData().result;
    	alert("Successfully: " + result);
    }
    catch( error )
    {
    	alert("Error: " + error);
    }
    ```

- PHP

    ```php
    try {
        $result = $serviceBuilder
            ->getBizProcScope()
            ->activity()
            ->update(
                'activity_code',
                'https://example.com/handler',
                1,
                ['en' => 'Activity Name', 'de' => 'Aktivitätsname'],
                ['en' => 'Activity Description', 'de' => 'Aktivitätsbeschreibung'],
                true,
                ['param1' => 'value1'],
                false,
                ['returnParam1' => 'value1'],
                null,
                null
            );

        if ($result->isSuccess()) {
            print($result->getCoreResponse()->getResponseData()->getResult()[0]);
        } else {
            print('Update failed.');
        }
    } catch (Throwable $e) {
        print('Error: ' . $e->getMessage());
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'bizproc.activity.update',
        {
            'CODE': 'action_test_code',
            'FIELDS': {
                'AUTH_USER_ID': 1,
                'USE_SUBSCRIPTION': 'N',
                'FILTER': {
                    'INCLUDE': [
                        ['lists'],
                        ['crm', 'CCrmDocumentDeal']
                    ]
                }
            },
        },
        function(result)
        {
            if(result.error())
                alert("Error: " + result.error());
            else
                alert("Successfully: " + result.data());
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'bizproc.activity.update',
        [
            'CODE' => 'action_test_code',
            'FIELDS' => [
                'AUTH_USER_ID' => 1,
                'USE_SUBSCRIPTION' => 'N',
                'FILTER' => [
                    'INCLUDE' => [
                        ['lists'],
                        ['crm', 'CCrmDocumentDeal']
                    ]
                ]
            ]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": true,
    "time": {
        "start": 1738149954.2918739,
        "finish": 1738149954.4590819,
        "duration": 0.16720795631408691,
        "processing": 0.017282962799072266,
        "date_start": "2025-01-29T14:25:54+01:00",
        "date_finish": "2025-01-29T14:25:54+01:00",
        "operating_reset_at": 1738150554,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../data-types.md) | Returns `true` if the action was successfully updated ||
|| **time**
[`time`](../../data-types.md#time) | Information about the execution time of the request ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "ERROR_ACTIVITY_VALIDATION_FAILURE",
    "error_description": "Wrong properties array!"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Error Message** | **Description** ||
|| `ACCESS_DENIED` | Application context required | Application context is required ||
|| `ACCESS_DENIED` | Access denied! | Method executed by a non-administrator ||
|| `ERROR_ACTIVITY_VALIDATION_FAILURE` | Empty activity code! | Action code not specified ||
|| `ERROR_ACTIVITY_VALIDATION_FAILURE` | Wrong activity code! | Invalid action code ||
|| `ERROR_ACTIVITY_NOT_FOUND` | Activity or Robot not found! | Action or robot not found ||
|| `ERROR_UNSUPPORTED_PROTOCOL` | Unsupported handler protocol | Invalid handler protocol http, https ||
|| `ERROR_WRONG_HANDLER_URL` | Wrong handler URL | Invalid handler URL ||
|| `ERROR_ACTIVITY_VALIDATION_FAILURE` | Wrong properties array! | Incorrectly filled parameters `PROPERTIES` or `RETURN_PROPERTIES` ||
|| `ERROR_ACTIVITY_VALIDATION_FAILURE` | Wrong property key <key>! | Invalid property identifier ||
|| `ERROR_ACTIVITY_VALIDATION_FAILURE` | Empty property NAME <key>! | Property name not specified ||
|| `ERROR_ACTIVITY_VALIDATION_FAILURE` | Wrong activity FILTER! | Invalid filter ||
|| `ERROR_ACTIVITY_VALIDATION_FAILURE` | Wrong activity DOCUMENT_TYPE! | Invalid `DOCUMENT_TYPE` ||
|| `ERROR_ACTIVITY_VALIDATION_FAILURE` | No fields to update | No fields to update ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./index.md)
- [{#T}](./bizproc-activity-add.md)
- [{#T}](./bizproc-activity-list.md)
- [{#T}](./bizproc-activity-delete.md)
- [{#T}](./bizproc-activity-log.md)