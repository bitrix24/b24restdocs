# Do this (brief essence of the method operation)

> Method name: **crm.xxx**
>
> Scope: [`crm`](../scopes/permissions.md)
>
> Who can execute the method: administrator / any user

The method does this

## Method parameters

{% include [Note on required parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **CODE***
[`string`](../../_includes/data-types.md) | Description of the parameter. The type refers to the page with standard data types ||
|| **NAME***
[`crm_item`](data-types.md) | Description of the parameter. The type refers to the page with data types of the current scope ||
|| **SETTINGS***
[`array`](../../data-types.md) | Example of a parameter with a complex nested structure. At this level, we describe it in general terms, but without all the details - just giving an overall idea. Because later, individual keys like CONFIG or ITEMS will be described in subsequent tables with separate subheadings.

```json
{
    "PRINT_URL": "value",
    "CHECK_URL": "value",
    "HTTP_VERSION": "value",
    "CONFIG":
    {
        "section_key_1": {
            "LABEL": "section name 1",
            "ITEMS": {
                "setting_1-1": {
                    "TYPE": "value",
                    "LABEL": "value"
                    ...
                },
                ...
                "setting_1-N": {
                    ...
                }
            }
        },
        ...
        "section_key_N": {
            "LABEL": "section name N",
            "ITEMS": {
                ...
            }
        }
    }
}
```

 ||
|#

### SETTINGS Parameter

{% include [Note on required parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **PRINT_URL***
[`string`](../../data-types.md) | Description of the parameter. The type refers to the page with standard data types  ||
|| **HTTP_VERSION***
[`http_status`](data-types.md) | Description of the parameter. The type refers to the page with data types of the current scope ||
|| **CONFIG***
[`array`](../../data-types.md) | Description of the parameter with a complex structure. Continuing from the previous table. Again, we provide a general overview, knowing that details about ITEMS will be below (we won't include ITEMS in the template anymore, the principle is clear)

```json
"section_key_1": {
    "LABEL": "section name 1",
    "ITEMS": {
        "setting_1-1": {
            ...
        },
        ... 
    }
},
...
"section_key_N": {
    ...
}
```

||
|| **TYPE***
[`string`](../../data-types.md) | Description of the parameter as a list of values (the same story for fields with `Y`/`N`). Possible values:

- `STRING` — string
- `NUMBER` — floating-point number
- `Y/N` — string that can take values `Y` or `N`
- `ENUM` — list of string values
- `DATE` — date

Default value: `STRING`
||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    Here we will insert the necessary code, regenerated from your JS example

- cURL (OAuth)

    Here we will insert the necessary code, regenerated from your JS example

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            "sale.cashbox.handler.add",
            {
                "CODE": "restcashbox01",
                "NAME": "REST-Cash Register 01",
                "SORT": 100,
                "SUPPORTS_FFD105": "Y",
                "SETTINGS":
                {
                    "PRINT_URL": "http://example.com/rest_print.php",
                    "CHECK_URL": "http://example.com/rest_check.php",
                    "HTTP_VERSION": "1.1",
                    "CONFIG":
                    {
                        "AUTH": {
                            "LABEL": "Authorization",
                            "ITEMS": {
                                "KEYWORD": {
                                    "TYPE": "STRING",
                                    "LABEL": "Password"
                                },
                                "PREFERENCE": {
                                    "TYPE": "ENUM",
                                    "LABEL": "Multiple choice",
                                    "REQUIRED": "Y",
                                    "OPTIONS": {
                                        "FIRST": "First",
                                        "SECOND": "Second",
                                        "THIRD": "Third",
                                    }
                                }
                            }
                        },
                        "INTERACTION": {
                            "LABEL": "Cash Register Interaction Settings",
                            "ITEMS": {
                                "MODE": {
                                    "TYPE": "ENUM",
                                    "LABEL": "Cash Register Operating Mode",
                                    "OPTIONS": {
                                        "ACTIVE": "active",
                                        "TEST": "test"
                                    }
                                }
                            }
                        }
                    }
                }
            }
        );
        
        const result = response.getData().result;
        console.dir(result);
    }
    catch(error)
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
                'sale.cashbox.handler.add',
                [
                    'CODE'          => 'restcashbox01',
                    'NAME'          => 'REST-Cash Register 01',
                    'SORT'          => 100,
                    'SUPPORTS_FFD105' => 'Y',
                    'SETTINGS'      => [
                        'PRINT_URL'    => 'http://example.com/rest_print.php',
                        'CHECK_URL'    => 'http://example.com/rest_check.php',
                        'HTTP_VERSION' => '1.1',
                        'CONFIG'       => [
                            'AUTH'       => [
                                'LABEL' => 'Authorization',
                                'ITEMS' => [
                                    'KEYWORD'    => [
                                        'TYPE'  => 'STRING',
                                        'LABEL' => 'Password',
                                    ],
                                    'PREFERENCE' => [
                                        'TYPE'     => 'ENUM',
                                        'LABEL'    => 'Multiple choice',
                                        'REQUIRED' => 'Y',
                                        'OPTIONS'  => [
                                            'FIRST'  => 'First',
                                            'SECOND' => 'Second',
                                            'THIRD'  => 'Third',
                                        ],
                                    ],
                                ],
                            ],
                            'INTERACTION' => [
                                'LABEL' => 'Cash Register Interaction Settings',
                                'ITEMS' => [
                                    'MODE' => [
                                        'TYPE'    => 'ENUM',
                                        'LABEL'   => 'Cash Register Operating Mode',
                                        'OPTIONS' => [
                                            'ACTIVE' => 'active',
                                            'TEST'   => 'test',
                                        ],
                                    ],
                                ],
                            ],
                        ],
                    ],
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
        // Your data processing logic
        processData($result);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error adding cash register handler: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "sale.cashbox.handler.add",
        {
            "CODE": "restcashbox01",
            "NAME": "REST-Cash Register 01",
            "SORT": 100,
            "SUPPORTS_FFD105": "Y",
            "SETTINGS":
            {
                "PRINT_URL": "http://example.com/rest_print.php",
                "CHECK_URL": "http://example.com/rest_check.php",
                "HTTP_VERSION": "1.1",
                "CONFIG":
                {
                    "AUTH": {
                        "LABEL": "Authorization",
                        "ITEMS": {
                            "KEYWORD": {
                                "TYPE": "STRING",
                                "LABEL": "Password"
                            },
                            "PREFERENCE": {
                                "TYPE": "ENUM",
                                "LABEL": "Multiple choice",
                                "REQUIRED": "Y",
                                "OPTIONS": {
                                    "FIRST": "First",
                                    "SECOND": "Second",
                                    "THIRD": "Third",
                                }
                            }
                        }
                    },
                    "INTERACTION": {
                        "LABEL": "Cash Register Interaction Settings",
                        "ITEMS": {
                            "MODE": {
                                "TYPE": "ENUM",
                                "LABEL": "Cash Register Operating Mode",
                                "OPTIONS": {
                                    "ACTIVE": "active",
                                    "TEST": "test"
                                }
                            }
                        }
                    }
                }
            }
        },
        function(result)
        {
            if(result.error())
                console.error(result.error());
            else
                console.dir(result.data());
        }
    );
    ```

- PHP CRest

    Here we will insert the necessary code, regenerated from your JS example

{% endlist %}

{% note tip "Typical use-cases and scenarios" %}

We will fill this block later. Or we will remove the block if unnecessary.

{% endnote %}

## Response Handling

HTTP status: **200**

```json
{
    "result": 5,
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
[`integer`](../../data-types.md) | Description of the returned value. A link either to the root reference of types or to data types within the scope ||
|| **time**
[`array`](../../data-types.md) | Information about the execution time of the request ||
|| **start**
[`double`](../../data-types.md) | Timestamp of the moment the request was initialized ||
|| **finish**
[`double`](../../data-types.md) | Timestamp of the moment the request execution was completed ||
|| **duration**
[`double`](../../data-types.md) | How long in milliseconds the request took (finish — start) ||
|| **date_start**
[`string`](../../data-types.md) | String representation of the date and time of the moment the request was initialized ||
|| **date_finish**
[`double`](../../data-types.md) | String representation of the date and time of the moment the request execution was completed ||
|| **operating_reset_at**
[`timestamp`](../../data-types.md) | Timestamp of the moment when the limit on REST API resources will be reset. Read more in the article [operation limits](../../../limits.md) ||
|| **operating**
[`double`](../../data-types.md) | In how many milliseconds the limit on REST API resources will be reset. Read more in the article [operation limits](../../../limits.md) ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "ERROR_HANDLER_ALREADY_EXIST",
    "error_description": "Handler already exists!"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `ERROR_CHECK_FAILURE` | Either a required field value is missing, or the value of one of the fields is incorrect ||
|| `ERROR_HANDLER_ALREADY_EXIST` | A handler with the code specified in the `CODE` parameter already exists in the system ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

We will fill this block later, but if you have recommendations on which methods or documentation pages should be mentioned here, we would appreciate it.