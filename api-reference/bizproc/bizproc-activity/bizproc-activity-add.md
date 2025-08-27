# Add New Action bizproc.activity.add

> Scope: [`bizproc`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

Adds a new action for use in workflows.

The method works only in the context of the [application](../../app-installation/index.md).

Each document generates its own set of field types. For example, in CRM there is a field of type Address `UF:address`. To use this field type in your actions, specify the CRM document type in `DOCUMENT_TYPE` and describe the properties of the type in `PROPERTIES`.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description**||
|| **CODE***
[`string`](../../data-types.md) | Internal identifier of the action. It is unique within the application.

Allowed characters are `a-z`, `A-Z`, `0-9`, dot, hyphen, and underscore `_` ||
|| **HANDLER***
[`string`](../../data-types.md) | URL to which the action will send data via the Bitrix24 queue server.

The link must have the same domain where the application is installed  ||
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
    'de': 'Aktion Name',
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
[`object`](../../data-types.md) | Object with action parameters. Contains objects, each of which describes a [parameter of the action](#property).

The system name of the parameter must start with a letter and can contain characters `a-z`, `A-Z`, `0-9`, and underscore `_` ||
|| **RETURN_PROPERTIES**
[`object`](../../data-types.md) | Object with additional results of the action. Contains objects, each of which describes a [parameter of the action](#property).

The parameter controls the ability of the action to wait for a response from the application and work with the data that will [come in the response](../bizproc-robot/bizproc-event-send.md).

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
[`object`](../../data-types.md) | Object with rules for limiting the action by document type and edition.

May contain keys:
- `INCLUDE` — array of rules where the action will be displayed
- `EXCLUDE` — array of rules where the action will be hidden

Each rule in the array can be a string or an array of document types in full or partial form.

To limit the action by Bitrix24 edition, specify:
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
[`boolean`](../../data-types.md) | Allows opening additional settings for the action in the application slider. Possible values:
- `Y` — yes
- `N` — no  ||
|#

### PROPERTY Object {#property}

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
        'de': 'Ordnungsnummer zuweisen',
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
    -d '{"CODE":"md5_action","HANDLER":"https://your_domain/ping.php","AUTH_USER_ID":1,"USE_SUBSCRIPTION":"Y","NAME":{"de":"MD5 Generator","en":"MD5 generator"},"DESCRIPTION":{"de":"Die Aktion gibt den MD5-Hash des Eingabeparameters zurück","en":"Activity returns MD5 hash of input parameter"},"PROPERTIES":{"inputString":{"Name":{"de":"Eingabestring","en":"Input string"},"Description":{"de":"Geben Sie den String ein, den Sie hashen möchten","en":"Input string for hashing"},"Type":"string","Required":"Y","Multiple":"N","Default":"{=Document:NAME}"}},"RETURN_PROPERTIES":{"outputString":{"Name":{"de":"MD5","en":"MD5"},"Type":"string","Multiple":"N","Default":null}},"DOCUMENT_TYPE":["lists","BizprocDocument","iblock_164"],"FILTER":{"INCLUDE":[["lists"]]},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/bizproc.activity.add
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'bizproc.activity.add',
    		{
    			'CODE': 'md5_action',
    			'HANDLER': 'https://your_domain/ping.php',
    			'AUTH_USER_ID': 1,
    			'USE_SUBSCRIPTION': 'Y',
    			'NAME': {
    				'de': 'MD5 Generator',
    				'en': 'MD5 generator'
    			},
    			'DESCRIPTION': {
    				'de': 'Die Aktion gibt den MD5-Hash des Eingabeparameters zurück',
    				'en': 'Activity returns MD5 hash of input parameter'
    			},
    			'PROPERTIES': {
    				'inputString': {
    					'Name': {
    						'de': 'Eingabestring',
    						'en': 'Input string'
    					},
    					'Description': {
    						'de': 'Geben Sie den String ein, den Sie hashen möchten',
    						'en': 'Input string for hashing'
    					},
    					'Type': 'string',
    					'Required': 'Y',
    					'Multiple': 'N',
    					'Default': '{=Document:NAME}'
    				}
    			},
    			'RETURN_PROPERTIES': {
    				'outputString': {
    					'Name': {
    						'de': 'MD5',
    						'en': 'MD5'
    					},
    					'Type': 'string',
    					'Multiple': 'N',
    					'Default': null
    				}
    			},
    			'DOCUMENT_TYPE': ['lists', 'BizprocDocument', 'iblock_164'],
    			'FILTER': {
    				INCLUDE: [
    					['lists']
    				]
    			}
    		}
    	);
    	
    	const result = response.getData().result;
    	alert("Success: " + result);
    }
    catch( error )
    {
    	alert("Error: " + error);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'bizproc.activity.add',
                [
                    'CODE'            => 'md5_action',
                    'HANDLER'         => 'https://your_domain/ping.php',
                    'AUTH_USER_ID'    => 1,
                    'USE_SUBSCRIPTION' => 'Y',
                    'NAME'            => [
                        'de' => 'MD5 Generator',
                        'en' => 'MD5 generator'
                    ],
                    'DESCRIPTION'     => [
                        'de' => 'Die Aktion gibt den MD5-Hash des Eingabeparameters zurück',
                        'en' => 'Activity returns MD5 hash of input parameter'
                    ],
                    'PROPERTIES'      => [
                        'inputString' => [
                            'Name'        => [
                                'de' => 'Eingabestring',
                                'en' => 'Input string'
                            ],
                            'Description' => [
                                'de' => 'Geben Sie den String ein, den Sie hashen möchten',
                                'en' => 'Input string for hashing'
                            ],
                            'Type'        => 'string',
                            'Required'    => 'Y',
                            'Multiple'    => 'N',
                            'Default'     => '{=Document:NAME}'
                        ]
                    ],
                    'RETURN_PROPERTIES' => [
                        'outputString' => [
                            'Name'     => [
                                'de' => 'MD5',
                                'en' => 'MD5'
                            ],
                            'Type'     => 'string',
                            'Multiple' => 'N',
                            'Default'  => null
                        ]
                    ],
                    'DOCUMENT_TYPE'    => ['lists', 'BizprocDocument', 'iblock_164'],
                    'FILTER'           => [
                        'INCLUDE' => [
                            ['lists']
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
        echo 'Error: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'bizproc.activity.add',
        {
            'CODE': 'md5_action',
            'HANDLER': 'https://your_domain/ping.php',
            'AUTH_USER_ID': 1,
            'USE_SUBSCRIPTION': 'Y',
            'NAME': {
                'de': 'MD5 Generator',
                'en': 'MD5 generator'
            },
            'DESCRIPTION': {
                'de': 'Die Aktion gibt den MD5-Hash des Eingabeparameters zurück',
                'en': 'Activity returns MD5 hash of input parameter'
            },
            'PROPERTIES': {
                'inputString': {
                    'Name': {
                        'de': 'Eingabestring',
                        'en': 'Input string'
                    },
                    'Description': {
                        'de': 'Geben Sie den String ein, den Sie hashen möchten',
                        'en': 'Input string for hashing'
                    },
                    'Type': 'string',
                    'Required': 'Y',
                    'Multiple': 'N',
                    'Default': '{=Document:NAME}'
                }
            },
            'RETURN_PROPERTIES': {
                'outputString': {
                    'Name': {
                        'de': 'MD5',
                        'en': 'MD5'
                    },
                    'Type': 'string',
                    'Multiple': 'N',
                    'Default': null
                }
            },
            'DOCUMENT_TYPE': ['lists', 'BizprocDocument', 'iblock_164'],
            'FILTER': {
                INCLUDE: [
                    ['lists']
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

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'bizproc.activity.add',
        [
            'CODE' => 'md5_action',
            'HANDLER' => 'https://your_domain/ping.php',
            'AUTH_USER_ID' => 1,
            'USE_SUBSCRIPTION' => 'Y',
            'NAME' => [
                'de' => 'MD5 Generator',
                'en' => 'MD5 generator'
            ],
            'DESCRIPTION' => [
                'de' => 'Die Aktion gibt den MD5-Hash des Eingabeparameters zurück',
                'en' => 'Activity returns MD5 hash of input parameter'
            ],
            'PROPERTIES' => [
                'inputString' => [
                    'Name' => [
                        'de' => 'Eingabestring',
                        'en' => 'Input string'
                    ],
                    'Description' => [
                        'de' => 'Geben Sie den String ein, den Sie hashen möchten',
                        'en' => 'Input string for hashing'
                    ],
                    'Type' => 'string',
                    'Required' => 'Y',
                    'Multiple' => 'N',
                    'Default' => '{=Document:NAME}'
                ]
            ],
            'RETURN_PROPERTIES' => [
                'outputString' => [
                    'Name' => [
                        'de' => 'MD5',
                        'en' => 'MD5'
                    ],
                    'Type' => 'string',
                    'Multiple' => 'N',
                    'Default' => null
                ]
            ],
            'DOCUMENT_TYPE' => ['lists', 'BizprocDocument', 'iblock_164'],
            'FILTER' => [
                'INCLUDE' => [
                    ['lists']
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

HTTP Status: **200**

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
[`boolean`](../../data-types.md) | Returns `true` if the action was successfully added ||
|| **time**
[`time`](../../data-types.md#time) | Information about the execution time of the request ||
|#

## Error Handling

HTTP Status: **400**

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
|| `ERROR_ACTIVITY_VALIDATION_FAILURE` | Empty data! | Required fields are not specified ||
|| `ERROR_ACTIVITY_VALIDATION_FAILURE` | Empty activity code! | Action code is not specified ||
|| `ERROR_ACTIVITY_VALIDATION_FAILURE` | Wrong activity code! | Invalid action code ||
|| `ERROR_UNSUPPORTED_PROTOCOL` | Unsupported handler protocol | Invalid handler protocol http, https ||
|| `ERROR_WRONG_HANDLER_URL` | Wrong handler URL | Invalid handler URL ||
|| `ERROR_ACTIVITY_VALIDATION_FAILURE` | Empty activity NAME! | Action name is not specified ||
|| `ERROR_ACTIVITY_VALIDATION_FAILURE` | Wrong properties array! | Incorrectly filled parameters `PROPERTIES` or `RETURN_PROPERTIES` ||
|| `ERROR_ACTIVITY_VALIDATION_FAILURE` | Wrong property key <key>! | Invalid property identifier ||
|| `ERROR_ACTIVITY_VALIDATION_FAILURE` | Empty property NAME <key>! | Property name is not specified ||
|| `ERROR_ACTIVITY_VALIDATION_FAILURE` | Wrong activity FILTER! | Invalid filter ||
|| `ERROR_ACTIVITY_VALIDATION_FAILURE` | Wrong activity DOCUMENT_TYPE! | Invalid `DOCUMENT_TYPE` ||
|| `ERROR_ACTIVITY_ALREADY_INSTALLED` | Activity or Robot already installed! | Action with this code is already installed ||
|| `ERROR_ACTIVITY_ADD_FAILURE` | Activity or Robot already added! | Action has already been added ||
|| `ERROR_ACTIVITY_ADD_FAILURE` | Activity save error! | Failed to save action, system error ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./index.md)
- [{#T}](./bizproc-activity-update.md)
- [{#T}](./bizproc-activity-list.md)
- [{#T}](./bizproc-activity-delete.md)
- [{#T}](./bizproc-activity-log.md)