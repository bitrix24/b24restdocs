# Add a New Action bizproc.activity.add

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits are needed to meet the writing standard
- parameters or fields are missing
- parameter types are not specified
- examples are missing
- response in case of success is missing
- response in case of error is missing
- links to pages that have not yet been created are not specified
- the method description is generally incorrect and needs to be rewritten
- parameters need to be split into several tables, considering that there are arrays inside
- incorrect footnote about required parameters

{% endnote %}

{% endif %}

> Scope: [`bizproc`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

Adds a new action for use in workflows.

Each document generates its own set of field types that it can work with. For example, CRM has a field of type Address, which is denoted as UF:address. To use such a field type in your activities, you need to specify that you are working with a CRM document (key `DOCUMENT_TYPE`), and then you can describe the properties of that type (key `PROPERTIES`).

#|
|| **Parameter** | **Description** ||

|| **CODE**^*^ | Internal identifier of the action, unique within the application. Allowed characters are a-z, A-Z, 0-9, dot, hyphen, and underscore.  ||
|| **HANDLER**^*^     | URL to which the action will send data (via the Bitrix24 queue server) when the workflow reaches its execution. Must refer to the same domain where the application is installed.  ||
|| **AUTH_USER_ID** | ID of the user whose token will be passed to the application. ||
|| **USE_SUBSCRIPTION** | Use of subscription. Allowed values - Y or N. Indicates whether the action should wait for a response from the application. If the parameter is empty or not specified, the user can configure this parameter in the action settings in the workflow designer.  ||
|| **NAME**^*^        | Name of the action. Can be a string or an associative array of localized strings. The value Title cannot be used. ||
|| **DESCRIPTION** | Description of the action. Can be a string or an associative array of localized strings. ||
|| **PROPERTIES**    | Array of action parameters. The list of values is similar to the values of the RETURN_PROPERTIES parameter.  ||
|| **RETURN_PROPERTIES** | Array of returned values of the action. The parameter controls whether the action can wait for a response from the application and work with the data that comes in the response.

{% note info "Attention!" %}

The system name of the parameter must start with a letter and can only contain characters a-z, A-Z, 0-9, and underscore.

{% endnote %}

Each parameter must contain: 
- Name - string or array of localizations. 
- Description - description of the parameter, string or array of localizations. 
- Type - type of the parameter. List of basic parameters: 
  - bool (Yes/No), 
  - date (Date), 
  - datetime (Date/Time), 
  - double (Number), 
  - int (Integer), 
  - select (List) array of list values, 
  - string (String), 
  - text (Text), 
  - user (User). 
- Options only for TYPE equal to select. 

```json
[
'value1' => 'title1',
'value2' => 'title2',
'value3' => 'title3',
'value4' => 'title4',
]
```

- Required(Y/N) - whether the parameter is required.
- Multiple(Y/N) - whether the parameter can have multiple values.
- Default - default value of the parameter. ||
|| **DOCUMENT_TYPE** | Type of document that will determine the data types for the PROPERTIES and RETURN_PROPERTIES parameters. An array of 3 elements: 
- module id,
- entity (class),
- document type itself.

Examples:

```php
['crm', 'CCrmDocumentLead', 'LEAD'], 
['lists', 'BizprocDocument', 'iblock_22'],
['disk', 'Bitrix\Disk\BizProcDocument', 'STORAGE_490'],
['tasks', 'Bitrix\Tasks\Integration\Bizproc\Document\Task', 'TASK_PROJECT_13'].
```

||
|| **FILTER**        | Rules for restricting the action by document type and edition. ||
|| **USE_PLACEMENT** | Allows opening additional settings for the action in the application slider. Accepts values (Y/N).  ||
|#

\* - required parameters

## Examples

{% list tabs %}

- JS

    ```js
    var params = {
        'CODE': 'md5',
        'HANDLER': 'http://example.com/ping.php',
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
        'DOCUMENT_TYPE': ['lists', 'BizprocDocument', 'iblock_1'],
        'FILTER': {
            INCLUDE: [
                ['lists']
            ]
        }
    };

    BX24.callMethod(
        'bizproc.activity.add',
        params,
        function(result)
        {
            if(result.error())
                alert("Error: " + result.error());
            else
                alert("Success: " + result.data());
        }
    );
    ```

{% endlist %}

Example parameters of the Workflow

```json
select
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
bool
'saveDoc': {
	'Name': {
		'de': 'Dokument speichern',
		'en': 'Save document'
	},
	'Description': {
		'de': 'Ordnen Sie eine fortlaufende Nummer zu',
		'en': 'Assign a sequential number'
	},
	'Type': 'bool',
	'Required': 'Y',
	'Multiple': 'N',
	'Default': 'Y'
}
string
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

{% include [Footnote on examples](../../../_includes/examples.md) %}