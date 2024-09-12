# Create Deals from Applications

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- adjustments needed for writing standards
- parameter types not specified
- parameter requirements not indicated

{% endnote %}

{% endif %}

Applications can create deals with a special type provider. Such a deal will have a corresponding [icon](*icon), it will be displayed in the timeline, and clicking on the deal will open the application in a slider with options in PLACEMENT_OPTIONS.

Deals of this subtype can only be modified or deleted in the context of the application that created them. Therefore, when updating such a deal using the method [crm.activity.update](../crm-activity-update.md) via webhook, an error will occur: `Access denied! Application context required`.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **PROVIDER_ID**
[`unknown`](../../../../data-types.md) | Provider identifier. For the special type, the value must be 'REST_APP'. ||
|| **PROVIDER_TYPE_ID**
[`unknown`](../../../../data-types.md)
| Deal type identifier. When using the 'REST_APP' provider, the developer can specify arbitrary type identifiers depending on their tasks. ||
|#

## Example

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
                        "SUBJECT": "New deal",
                        "COMPLETED": "N",
                        "RESPONSIBLE_ID": 1,
                        "DESCRIPTION": "Description of the new deal"
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
                        SUBJECT: "Deal completed!",
                        DESCRIPTION: "Description of the new deal (completed)"
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
                var    id = selected['lead'][0]['id'];

                selectedEntityId = id.substring(2);

                console.log(selectedEntityId);
            }
        })
    }
</script>
</body>
</html>
```

{% include [Examples Note](../../../../../_includes/examples.md) %}

[*icon]: ![icon](./_images/activity_application.png)