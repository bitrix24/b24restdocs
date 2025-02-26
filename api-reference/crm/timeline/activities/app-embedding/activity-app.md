# How to create an activity from an application

Applications can create activities with a special type provider. Such an activity will have a corresponding [icon](*icon) in the timeline. Clicking on the activity will open the application in a slider with options in [PLACEMENT_OPTIONS](../../../../widgets/crm/detail-activity.md#placement_options)

{% note warning %}

Activities with a special type provider can only be modified/deleted in the context of the application that created the activity. When updating such an activity using the [crm.activity.update](../activity-base/crm-activity-update.md) method via webhook, an error will occur: `Access denied! Application context required`.

{% endnote %}

## Parameters

{% include [Note on required parameters](../../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **PROVIDER_ID***  
[`string`](../../../../data-types.md) | Provider identifier. For special type, the value must be `REST_APP` ||
|| **PROVIDER_TYPE_ID***  
[`string`](../../../../data-types.md) | Activity type identifier. When using the `REST_APP` provider, the developer can specify arbitrary type identifiers depending on their tasks ||
|#

## Example application

{% include [Note on examples](../../../../../_includes/examples.md) %}

{% list tabs %}

- PHP

    ```php
    <?php
    header('Content-Type: text/html; charset=UTF-8');
    ?>
    <!DOCTYPE html>
    <html>
    <head>
        <meta charset="UTF-8">
        <title></title>
        <style type="text/css">

        </style>
    </head>
    <body style="display: none">
    <script src="//api.bitrix24.com/api/v1/"></script>

    <?if (isset($_POST['PLACEMENT']) && !empty($_POST['PLACEMENT_OPTIONS'])):
        $opt = json_decode($_POST['PLACEMENT_OPTIONS'], true);
    ?>
    <p>Activity ID: <?= (int)$opt['activity_id']?></p>
    <button onclick="updateActivity(<?= (int)$opt['activity_id']?>);">Update activity (set new description + completed)</button>
    <p><button onclick="deleteActivity(<?= (int)$opt['activity_id']?>);">Delete activity</button>
    <?else:?>
    <button onclick="selectCRMEntity();">Select LEAD</button>
    <span id="selected-entity"></span>
    <p>
    <button onclick="addActivity();">Add activity</button>
    <?endif;?>
    <script type="text/javascript">
        BX24.init(function()
        {
            document.body.style.display = '';
        });

        var selectedEntityId = null;

        function addActivity()
        {

            if (!selectedEntityId)
            {
                alert('Lead not selected');
                return;
            }
            BX24.callMethod(
                'crm.activity.add',
                {
                    fields:
                        {
                            "OWNER_TYPE_ID": 1,
                            "OWNER_ID": selectedEntityId,
                            "PROVIDER_ID": 'REST_APP',
                            "PROVIDER_TYPE_ID": 'LINK',
                            "SUBJECT": "New activity",
                            "COMPLETED": "N",
                            "RESPONSIBLE_ID": 1,
                            "DESCRIPTION": "Description of the new activity"
                        }
                },
                function(result)
                {
                    if(result.error())
                        alert("Error: " + result.error());
                    else
                    {
                        alert("Success: " + result.data());
                    }
                }
            );
        }
        function updateActivity(id)
        {
            BX24.callMethod(
                'crm.activity.update',
                {
                    id: id,
                    fields:
                        {
                            COMPLETED: 'Y',
                            SUBJECT: "Activity completed!",
                            DESCRIPTION: "Description of the new activity (completed)"
                        }
                },
                function(result)
                {
                    if(result.error())
                        alert("Error: " + result.error());
                    else
                    {
                        alert("Success: " + result.data());
                    }
                }
            );
        }

        function deleteActivity(id)
        {
            BX24.callMethod(
                'crm.activity.delete',
                {
                    id: id
                },
                function(result)
                {
                    if(result.error())
                        alert("Error: " + result.error());
                    else
                    {
                        alert("Success: " + result.data());
                    }
                }
            );
        }

        function selectCRMEntity()
        {
            document.getElementById('selected-entity').textContent = '';
            BX24.selectCRM({
                entityType: ['lead']
            }, function(selected)
            {
                if (selected['lead'] && selected['lead'][0])
                {
                    document.getElementById('selected-entity').textContent = selected['lead'][0]['title'];
                    var id = selected['lead'][0]['id'];

                    selectedEntityId = id.substring(2);

                    console.log(selectedEntityId);
                }
            })
        }
    </script>
    </body>
    </html>
    ```

{% endlist %}

[*icon]: ![icon](./_images/activity_application.png)