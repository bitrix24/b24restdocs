# Set Up REST Automation Rules (Actions) via Application (Widget)

In *Bitrix24*, you can configure an Automation rule/action using the application interface. This is implemented through the standard [widget embedding mechanism](../../api-reference/widgets/index.md). In the on-premise version, it is available starting from version [20.0.600](../../api-reference/cloud-and-on-premise/on-premise/versions.md) of the `Business Processes` module.

## Implementation Using an Application Example

Let's consider the implementation using a specific application example (the full code of the application is presented [below](#full-application-code)).

In our example, the application adds an Automation rule that has 2 parameters of type `string`.

{% include [Example Notes](../../_includes/examples.md) %}

{% list tabs %}

- JS

    ```js
    var params = {
        'CODE': 'robot',
        'HANDLER': 'http://handler.com',
        'AUTH_USER_ID': 1,
        'NAME': 'Example Automation Rule Widget',
        'USE_PLACEMENT': 'Y',
        'PLACEMENT_HANDLER': 'http://handler.com',
        'PROPERTIES': {
            'string': {
                'Name': 'Parameter 1',
                'Type': 'string'
            },
            'stringm': {
                'Name': 'Parameter 2',
                'Type': 'string',
                'Multiple': 'Y',
                'Default': ['value 1', 'value 2']
            },
        }
    };
    BX24.callMethod(
        'bizproc.robot.add',
        params,
        function(result)
        {
            if(result.error())
                alert("Error: " + result.error());
        }
    );
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'bizproc.robot.add',
        [
            'CODE' => 'robot',
            'HANDLER' => 'http://handler.com',
            'AUTH_USER_ID' => 1,
            'NAME' => 'Example Automation Rule Widget',
            'USE_PLACEMENT' => 'Y',
            'PLACEMENT_HANDLER' => 'http://handler.com',
            'PROPERTIES' => [
                'string' => [
                    'Name' => 'Parameter 1',
                    'Type' => 'string'
                ],
                'stringm' => [
                    'Name' => 'Parameter 2',
                    'Type' => 'string',
                    'Multiple' => 'Y',
                    'Default' => ['value 1', 'value 2']
                ],
            ]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

To allow parameters to be configured through the application, when adding the Automation rule, you need to pass the parameters `USE_PLACEMENT=Y` and the handler `PLACEMENT_HANDLER`.

Next, you need to write the embedding handler that will render the parameters and save their values. For this, you need to pass the following data in `PLACEMENT_OPTIONS` to the handler:

- `code` — the code of your Automation rule upon registration
- `activity_name` — the identifier of the action in the workflow template
- `properties` — a list of properties and their descriptions
- `current_values` — the current values of the properties
- `document_type` — the type of document for which the configuration is being set
- `document_fields` — a list of document fields
- `template` — a list of available template fields (in the on-premise version of *Bitrix24*, available starting from version [24.200.0](../../api-reference/cloud-and-on-premise/on-premise/versions.md))

    For the `template` parameter, the following elements are available:

    - `parameters` — a list of template parameters
    - `variables` — a list of template variables
    - `constants` — a list of template constants
    - `global_variables` — a list of global variables
    - `global_constants` — a list of global constants
    - `return_activities` — a list of actions (Automation rules) that generate additional results

    The structure of properties in these lists is unified and represented as follows:

    ```
    {
        Id: string, property identifier (code)
        Type: string, property type identifier

        Name: string, name
        Description: string, description

        Multiple: boolean, whether it is a multiple property
        Required: boolean, whether it is a required property

        Options: mixed. Depends on the property type
        Settings: list of settings. Depends on the property type
        Default: mixed. Default value for the property
    }
    ```

    The `return_activities` list consists of a list of actions that return results and their properties. It has the following structure:

    ```
    {
        Id: string, action identifier in the template
        Type: string, action type
        Title: string, action title in the template
        Return: list, available properties
    }
    ```

In the application, all this comes in the following format:

{% list tabs %}

- JS

    ```js
    Array
    (
        [code] => robot
        [activity_name] => A72788_31169_37133_27365
        [properties] => Array
            (
                [string] => Array
                    (
                        [NAME] => Parameter 1
                        [TYPE] => string
                    )
                [stringm] => Array
                    (
                        [NAME] => Parameter 2
                        [TYPE] => string
                        [MULTIPLE] => Y
                        [DEFAULT] => Array
                            (
                                [0] => value 1
                                [1] => value 2
                            )
                    )
            )
        [current_values] => Array
            (
                [string] => 1
                [stringm] => Array
                    (
                        [0] => 2
                    )
            )
        [document_type] => Array
            (
                [0] => crm
                [1] => CCrmDocumentDeal
                [2] => DEAL
            )
        [document_fields] => Array
            (
                [ID] => Array
                    (
                        [Name] => ID
                        [Type] => int
                        [Filterable] => 1
                        [Editable] => 
                        [Required] => 
                        [BaseType] => int
                    )
                [TITLE] => Array
                    (
                        [Name] => Title
                        [Type] => string
                        [Filterable] => 1
                        [Editable] => 1
                        [Required] => 1
                        [BaseType] => string
                    )
                    
                [...]	
            )
        [template] => Array
            (
                [parameters] => Array
                    (
                    )
                [variables] => Array
                    (
                        [0] => Array
                            (
                                [Id] => Approver
                                [Type] => user
                                [Name] => Approver
                                [Description] => 
                                [Multiple] => 1
                                [Required] => 
                                [Options] => 
                                [Settings] => 
                                [Default] => 
                            )
                    )
                [constants] => Array
                    (
                        [0] => Array
                            (
                                [Id] => Manager
                                [Type] => user
                                [Name] => Who Approves
                                [Description] => director or deputy
                                [Multiple] => 1
                                [Required] => 1
                                [Options] => 
                                [Settings] => 
                                [Default] => Array
                                    (
                                        [0] => user_4
                                    )
                            )
                    )
                [global_variables] => Array
                    (
                        [0] => Array
                            (
                                [Id] => Variable1666332520655
                                [Type] => user
                                [Name] => test u
                                [Description] => 
                                [Multiple] => 1
                                [Required] => 
                                [Options] => 
                                [Settings] => 
                                [Default] => Array
                                    (
                                        [0] => user_1
                                    )
                            )
                    )
                [global_constants] => Array
                    (
                        [0] => Array
                            (
                                [Id] => Constant1666332578194
                                [Type] => user
                                [Name] => test u 1
                                [Description] => 
                                [Multiple] => 
                                [Required] => 
                                [Options] => 
                                [Settings] => 
                                [Default] => user_1
                            )
                    )
                [return_activities] => Array
                    (
                        [0] => Array
                            (
                                [Id] => A71026_84473_24610_19894
                                [Type] => LogActivity
                                [Title] => Log to Report
                                [Return] => Array
                                    (
                                        [0] => Array
                                            (
                                                [Id] => Report
                                                [Name] => Report
                                                [Type] => string
                                            )
                                    )
                            )
                        [1] => Array
                            (
                                [Id] => A58853_60082_34258_61777
                                [Type] => ApproveActivity
                                [Title] => Approval
                                [Return] => Array
                                    (
                                        [0] => Array
                                            (
                                                [Id] => TaskId
                                                [Name] => ID
                                                [Type] => int
                                            )
                                        [1] => Array
                                            (
                                                [Id] => Comments
                                                [Name] => Comments
                                                [Type] => string
                                            )
                                        [2] => Array
                                            (
                                                [Id] => VotedCount
                                                [Name] => Number of Votes
                                                [Type] => int
                                            )
                                        [3] => Array
                                            (
                                                [Id] => TotalCount
                                                [Name] => Total Votes Required
                                                [Type] => int
                                            )
                                        [4] => Array
                                            (
                                                [Id] => VotedPercent
                                                [Name] => Voting Percentage
                                                [Type] => int
                                            )
                                        [5] => Array
                                            (
                                                [Id] => ApprovedPercent
                                                [Name] => Approval Percentage
                                                [Type] => int
                                            )
                                        [6] => Array
                                            (
                                                [Id] => NotApprovedPercent
                                                [Name] => Rejection Percentage
                                                [Type] => int
                                            )
                                        [7] => Array
                                            (
                                                [Id] => ApprovedCount
                                                [Name] => Number Approved
                                                [Type] => int
                                            )
                                        [8] => Array
                                            (
                                                [Id] => NotApprovedCount
                                                [Name] => Number Rejected
                                                [Type] => int
                                            )
                                        [9] => Array
                                            (
                                                [Id] => LastApprover
                                                [Name] => Last Voter
                                                [Type] => user
                                            )
                                        [10] => Array
                                            (
                                                [Id] => LastApproverComment
                                                [Name] => Last Voter Comment
                                                [Type] => string
                                            )
                                        [11] => Array
                                            (
                                                [Id] => UserApprovers
                                                [Name] => Users Who Approved
                                                [Type] => user
                                            )
                                        [12] => Array
                                            (
                                                [Id] => Approvers
                                                [Name] => Users Who Approved (text)
                                                [Type] => string
                                            )
                                        [13] => Array
                                            (
                                                [Id] => UserRejecters
                                                [Name] => Users Who Rejected
                                                [Type] => user
                                            )

                                        [14] => Array
                                            (
                                                [Id] => Rejecters
                                                [Name] => Users Who Rejected (text)
                                                [Type] => string
                                            )
                                        [15] => Array
                                            (
                                                [Id] => IsTimeout
                                                [Name] => Automatic Rejection
                                                [Type] => int
                                            )
                                    )
                            )
                    )
            )
    )
    ```

- PHP

    ```php
    $array = [
        'code' => 'robot',
        'activity_name' => 'A72788_31169_37133_27365',
        'properties' => [
            'string' => [
                'NAME' => 'Parameter 1',
                'TYPE' => 'string'
            ],
            'stringm' => [
                'NAME' => 'Parameter 2',
                'TYPE' => 'string',
                'MULTIPLE' => 'Y',
                'DEFAULT' => [
                    'value 1',
                    'value 2'
                ]
            ]
        ],
        'current_values' => [
            'string' => 1,
            'stringm' => [
                2
            ]
        ],
        'document_type' => [
            'crm',
            'CCrmDocumentDeal',
            'DEAL'
        ],
        'document_fields' => [
            'ID' => [
                'Name' => 'ID',
                'Type' => 'int',
                'Filterable' => 1,
                'Editable' => false,
                'Required' => false,
                'BaseType' => 'int'
            ],
            'TITLE' => [
                'Name' => 'Title',
                'Type' => 'string',
                'Filterable' => 1,
                'Editable' => 1,
                'Required' => 1,
                'BaseType' => 'string'
            ],
            // ...
        ],
        'template' => [
            'parameters' => [],
            'variables' => [
                [
                    'Id' => 'Approver',
                    'Type' => 'user',
                    'Name' => 'Approver',
                    'Description' => '',
                    'Multiple' => 1,
                    'Required' => false,
                    'Options' => '',
                    'Settings' => '',
                    'Default' => ''
                ]
            ],
            'constants' => [
                [
                    'Id' => 'Manager',
                    'Type' => 'user',
                    'Name' => 'Who Approves',
                    'Description' => 'director or deputy',
                    'Multiple' => 1,
                    'Required' => 1,
                    'Options' => '',
                    'Settings' => '',
                    'Default' => [
                        'user_4'
                    ]
                ]
            ],
            'global_variables' => [
                [
                    'Id' => 'Variable1666332520655',
                    'Type' => 'user',
                    'Name' => 'test u',
                    'Description' => '',
                    'Multiple' => 1,
                    'Required' => false,
                    'Options' => '',
                    'Settings' => '',
                    'Default' => [
                        'user_1'
                    ]
                ]
            ],
            'global_constants' => [
                [
                    'Id' => 'Constant1666332578194',
                    'Type' => 'user',
                    'Name' => 'test u 1',
                    'Description' => '',
                    'Multiple' => false,
                    'Required' => false,
                    'Options' => '',
                    'Settings' => '',
                    'Default' => 'user_1'
                ]
            ],
            'return_activities' => [
                [
                    'Id' => 'A71026_84473_24610_19894',
                    'Type' => 'LogActivity',
                    'Title' => 'Log to Report',
                    'Return' => [
                        [
                            'Id' => 'Report',
                            'Name' => 'Report',
                            'Type' => 'string'
                        ]
                    ]
                ],
                [
                    'Id' => 'A58853_60082_34258_61777',
                    'Type' => 'ApproveActivity',
                    'Title' => 'Approval',
                    'Return' => [
                        [
                            'Id' => 'TaskId',
                            'Name' => 'ID',
                            'Type' => 'int'
                        ],
                        [
                            'Id' => 'Comments',
                            'Name' => 'Comments',
                            'Type' => 'string'
                        ],
                        [
                            'Id' => 'VotedCount',
                            'Name' => 'Number of Votes',
                            'Type' => 'int'
                        ],
                        [
                            'Id' => 'TotalCount',
                            'Name' => 'Total Votes Required',
                            'Type' => 'int'
                        ],
                        [
                            'Id' => 'VotedPercent',
                            'Name' => 'Voting Percentage',
                            'Type' => 'int'
                        ],
                        [
                            'Id' => 'ApprovedPercent',
                            'Name' => 'Approval Percentage',
                            'Type' => 'int'
                        ],
                        [
                            'Id' => 'NotApprovedPercent',
                            'Name' => 'Rejection Percentage',
                            'Type' => 'int'
                        ],
                        [
                            'Id' => 'ApprovedCount',
                            'Name' => 'Number Approved',
                            'Type' => 'int'
                        ],
                        [
                            'Id' => 'NotApprovedCount',
                            'Name' => 'Number Rejected',
                            'Type' => 'int'
                        ],
                        [
                            'Id' => 'LastApprover',
                            'Name' => 'Last Voter',
                            'Type' => 'user'
                        ],
                        [
                            'Id' => 'LastApproverComment',
                            'Name' => 'Last Voter Comment',
                            'Type' => 'string'
                        ],
                        [
                            'Id' => 'UserApprovers',
                            'Name' => 'Users Who Approved',
                            'Type' => 'user'
                        ],
                        [
                            'Id' => 'Approvers',
                            'Name' => 'Users Who Approved (text)',
                            'Type' => 'string'
                        ],
                        [
                            'Id' => 'UserRejecters',
                            'Name' => 'Users Who Rejected',
                            'Type' => 'user'
                        ],
                        [
                            'Id' => 'Rejecters',
                            'Name' => 'Users Who Rejected (text)',
                            'Type' => 'string'
                        ],
                        [
                            'Id' => 'IsTimeout',
                            'Name' => 'Automatic Rejection',
                            'Type' => 'int'
                        ]
                    ]
                ]
            ]
        ]
    ];
    ```

{% endlist %}

Next, the user saves the Automation rule as usual.

The white area is the widget (application frame).

## Full Application Code

{% include [Example Notes](../../_includes/examples.md) %}

{% list tabs %}

- JS

    ```js
    let protocol = location.protocol === 'https:' ? 'https' : 'http';
    let host = location.hostname;
    const BP_APP_HANDLER = `${protocol}://${host}${location.pathname}`;

    document.write(`
    <!DOCTYPE html>
    <html lang="en">
    <head>
    <meta charset="UTF-8">
    <title></title>
    </head>
    <body>
    <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/css/bootstrap.min.css" integrity="sha384-ggOyR0iXCbMQv3Xipma34MD+dH/1fQ784/j6cY/iJTQUOhcWr7x9JvoRxT2MZw1T" crossorigin="anonymous">
    <script src="https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/js/bootstrap.min.js" integrity="sha384-JjSmVgyd0p3pXB1rRibZUAYoIIy6OrQ6VrjIEaFf/nJGzIxFDsf4x0xIM+B07jRM" crossorigin="anonymous"></script>
    <script src="//api.bitrix24.com/api/v1/"></script>
    ${!BX24.getParam('PLACEMENT') || BX24.getParam('PLACEMENT') === 'DEFAULT' ? `
    <h1>Automation Rule Widget</h1>
    <div class="container-fluid">
    <div class="container-fluid">
            <h2>Automation Rule</h2>
            <button onclick="installRobot();" class="btn btn-primary">Install</button>
            <button onclick="uninstallRobot();" class="btn btn-danger">Remove</button>
        </div>
        <hr/>
        <div class="container-fluid">
            <button onclick="getList();" class="btn btn-light">Get List of Installed Automation Rules</button>
        </div>
    </div>
    <script type="text/javascript">
        document.body.style.display = 'none';
        BX24.init(function()
        {
            document.body.style.display = '';
        });
        function installRobot()
        {
            var params = {
                'CODE': 'robot',
                'HANDLER': '${BP_APP_HANDLER}',
                'AUTH_USER_ID': 1,
                'NAME': 'Example Automation Rule Widget',
                'USE_PLACEMENT': 'Y',
                'PLACEMENT_HANDLER': '${BP_APP_HANDLER}',
                'PROPERTIES': {
                    'string': {
                        'Name': 'Parameter 1',
                        'Type': 'string'
                    },
                    'stringm': {
                        'Name': 'Parameter 2',
                        'Type': 'string',
                        'Multiple': 'Y',
                        'Default': ['value 1', 'value 2']
                    },
                }
            };
            BX24.callMethod(
                'bizproc.robot.add',
                params,
                function(result)
                {
                    if(result.error())
                        alert("Error: " + result.error());
                    else
                        alert("Successfully Installed");
                }
            );
        }
        function uninstallRobot()
        {
            BX24.callMethod(
                    'bizproc.robot.delete',
                    {
                        'CODE': 'robot'
                    },
                    function(result)
                    {
                        if(result.error())
                            alert('Error: ' + result.error());
                        else
                            alert("Successfully Removed");
                    }
            );
        }
        function getList()
        {
            BX24.callMethod(
                'bizproc.robot.list',
                {},
                function(result)
                {
                    if(result.error())
                        alert("Error: " + result.error());
                    else
                        alert("Codes of Installed Automation Rules: " + result.data().join(', '));
                }
            );
        }
    </script>
    ` : `
    <form name="props" class="container-fluid">
    ${(() => {
        let options = JSON.parse(BX24.getParam('PLACEMENT_OPTIONS'));
        return Object.keys(options.properties).map(id => {
            let property = options.properties[id];
            let multiple = property.MULTIPLE === 'Y';
            let val = options.current_values[id] || [''];
            if (multiple) val.push('');
            let name = multiple ? `${id}[]` : id;
            return `
            <div class="form-group">
                <label>${property.NAME}:</label>
                ${val.map(v => `<p><input name="${name}" value="${v}" class="form-control" onchange="setPropertyValue('${id}', this.name, ${multiple})"></p>`).join('')}
            </div>`;
        }).join('');
    })()}
    <script>
        function setPropertyValue(name, inputName, multiple)
        {
            var form = new FormData(document.forms.props);
            var value = multiple ? form.getAll(inputName) : form.get(inputName);
            var params = {};
            params[name] = value;
            BX24.placement.call(
                'setPropertyValue',
                params
            )
        }
    </script>
    </form>
    `}
    </body>
    </html>
    `);
    ```

- PHP

    ```php
    <?php
    header('Content-Type: text/html; charset=UTF-8');
    $protocol = $_SERVER['SERVER_PORT'] == '443' ? 'https' : 'http';
    $host = explode(':', $_SERVER['HTTP_HOST']);
    $host = $host[0];
    define('BP_APP_HANDLER', $protocol.'://'.$host.explode('?', $_SERVER['REQUEST_URI'])[0]);
    ?>
    <!DOCTYPE html>
    <html lang="en">
    <head>
    <meta charset="UTF-8">
    <title></title>
    </head>
    <body>
    <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/css/bootstrap.min.css" integrity="sha384-ggOyR0iXCbMQv3Xipma34MD+dH/1fQ784/j6cY/iJTQUOhcWr7x9JvoRxT2MZw1T" crossorigin="anonymous">
    <script src="https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/js/bootstrap.min.js" integrity="sha384-JjSmVgyd0p3pXB1rRibZUAYoIIy6OrQ6VrjIEaFf/nJGzIxFDsf4x0xIM+B07jRM" crossorigin="anonymous"></script>
    <script src="//api.bitrix24.com/api/v1/"></script>
    <?if (!isset($_POST['PLACEMENT']) || $_POST['PLACEMENT'] === 'DEFAULT'):?>
    <h1>Automation Rule Widget</h1>
    <div class="container-fluid">
    <div class="container-fluid">
            <h2>Automation Rule</h2>
            <button onclick="installRobot();" class="btn btn-primary">Install</button>
            <button onclick="uninstallRobot();" class="btn btn-danger">Remove</button>
        </div>
        <hr/>
        <div class="container-fluid">
            <button onclick="getList();" class="btn btn-light">Get List of Installed Automation Rules</button>
        </div>
    </div>
    <script type="text/javascript">
        document.body.style.display = 'none';
        BX24.init(function()
        {
            document.body.style.display = '';
        });
        function installRobot()
        {
            var params = {
                'CODE': 'robot',
                'HANDLER': '<?=BP_APP_HANDLER?>',
                'AUTH_USER_ID': 1,
                'NAME': 'Example Automation Rule Widget',
                'USE_PLACEMENT': 'Y',
                'PLACEMENT_HANDLER': '<?=BP_APP_HANDLER?>',
                'PROPERTIES': {
                    'string': {
                        'Name': 'Parameter 1',
                        'Type': 'string'
                    },
                    'stringm': {
                        'Name': 'Parameter 2',
                        'Type': 'string',
                        'Multiple': 'Y',
                        'Default': ['value 1', 'value 2']
                    },
                }
            };
            BX24.callMethod(
                'bizproc.robot.add',
                params,
                function(result)
                {
                    if(result.error())
                        alert("Error: " + result.error());
                    else
                        alert("Successfully Installed");
                }
            );
        }
        function uninstallRobot()
        {
            BX24.callMethod(
                    'bizproc.robot.delete',
                    {
                        'CODE': 'robot'
                    },
                    function(result)
                    {
                        if(result.error())
                            alert('Error: ' + result.error());
                        else
                            alert("Successfully Removed");
                    }
            );
        }
        function getList()
        {
            BX24.callMethod(
                'bizproc.robot.list',
                {},
                function(result)
                {
                    if(result.error())
                        alert("Error: " + result.error());
                    else
                        alert("Codes of Installed Automation Rules: " + result.data().join(', '));
                }
            );
        }
    </script>
    <?php else:?>
        <form name="props" class="container-fluid">
    <?php
    $options = json_decode($_POST['PLACEMENT_OPTIONS'], true);
    foreach ($options['properties'] as $id => $property)
    {
        $multiple = isset($property['MULTIPLE']) && $property['MULTIPLE'] === 'Y';
        $val = (array) $options['current_values'][$id];
        if (!$val)
        {
            $val[] = '';
        }
        if ($multiple)
        {
            $val[] = '';
        }
        $name = $multiple ? $id.'[]' : $id;
        ?>
        <div class="form-group">
            <label><?=htmlspecialchars($property['NAME'])?>:</label>
            <?foreach ($val as $v):?>
            <p><input name="<?=$name?>" value="<?=htmlspecialchars((string)$v)?>" class="form-control" onchange="setPropertyValue('<?=$id?>', this.name, <?=(int)$multiple?>)"></p>
            <?endforeach;?>
        </div>
        <?
    }
    ?>
            <script>
                function setPropertyValue(name, inputName, multiple)
                {
                    var form = new FormData(document.forms.props);
                    var value = multiple? form.getAll(inputName) : form.get(inputName);
                    var params = {};
                    params[name] = value;
                    BX24.placement.call(
                        'setPropertyValue',
                        params
                    )
                }
            </script>
        </form>
    <?php endif;?>
    </body>
    </html>
    ```

{% endlist %}