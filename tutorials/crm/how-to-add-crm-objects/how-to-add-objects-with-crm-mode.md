# Add a lead or deal with an activity considering the CRM mode

> Scope: [`crm`](../../../api-reference/scopes/permissions.md)
>
> Who can execute the method: users with administrative access to the CRM section

If the CRM operates in simple mode, an activity is added to the deal; if in classic mode — to the lead.

- Create a form on the desired page:

```html
<script src="https://ajax.googleapis.com/ajax/libs/jquery/3.3.1/jquery.min.js"></script>
<script>
$(document).ready(function() {
    $('#form_to_crm').on('submit', function(el) { // event submit form
        el.preventDefault(); // the default action of the event will not be triggered
        var formData = $(this).serialize();
        $.ajax({
            'method': 'POST',
            'dataType': 'json',
            'url': 'form.php', // file for saving filled forms
            'data': formData,
            success: function(data) { // success callback
                alert(data.message);
            }
        });
    });
});
</script>
    
<form id="form_to_crm">
    <input type="text" name="NAME" placeholder="Name" required>
    <input type="text" name="LAST_NAME" placeholder="Last name">
    <input type="text" name="PHONE" placeholder="Phone">
    <input type="text" name="EMAIL" placeholder="E-mail">
    <input type="submit" value="Submit">
</form>
```

- Create a file for saving filled forms:

{% list tabs %}

- JS

    ```js
    document.addEventListener('DOMContentLoaded', function() {
        document.getElementById('form_to_crm').addEventListener('submit', function(el) {
            el.preventDefault();
            let formData = new FormData(this);
            let sName = formData.get("NAME");
            let sLastName = formData.get("LAST_NAME");
            let sPhone = formData.get("PHONE");
            let sEmail = formData.get("EMAIL");

            let arData = {
                'add_lead': {
                    'method': 'crm.lead.add',
                    'params': {
                        'fields': {
                            'TITLE': 'From the site: ' + [sName, sLastName].join(' '),
                            'NAME': sName || 'Empty name',
                            'LAST_NAME': sLastName,
                            'PHONE': sPhone ? [{ 'VALUE': sPhone, 'VALUE_TYPE': 'HOME' }] : [],
                            'EMAIL': sEmail ? [{ 'VALUE': sEmail, 'VALUE_TYPE': 'HOME' }] : []
                        }
                    }
                },
                'get_lead': {
                    'method': 'crm.lead.get',
                    'params': {
                        'id': '$result[add_lead]'
                    }
                }
            };

            BX24.callBatch(arData, function(result) {
                if (!result.result.result_error.add_lead && result.result.result.get_lead) {
                    if (result.result.result.get_lead.STATUS_ID === 'CONVERTED') {
                        BX24.callMethod(
                            'crm.deal.list',
                            {
                                'filter': {
                                    'LEAD_ID': result.result.result.add_lead
                                }
                            },
                            function(resultDeal) {
                                if (resultDeal.data().length > 0) {
                                    BX24.callMethod(
                                        'crm.activity.add',
                                        {
                                            'fields': {
                                                "OWNER_TYPE_ID": 2,
                                                "TYPE_ID": 2,
                                                "OWNER_ID": resultDeal.data()[0].ID,
                                                "COMMUNICATIONS": [{ 'VALUE': sPhone, 'TYPE': 'PHONE' }],
                                                "START_TIME": new Date().toISOString(),
                                                "END_TIME": new Date(Date.now() + 3600 * 1000).toISOString(),
                                                "SUBJECT": "Call back",
                                                "DESCRIPTION": "Call within an hour"
                                            }
                                        },
                                        function(resultActivity) {
                                            if (resultActivity.error()) {
                                                console.error(resultActivity.error());
                                            }
                                        }
                                    );
                                }
                            }
                        );
                    } else {
                        BX24.callMethod(
                            'crm.activity.add',
                            {
                                'fields': {
                                    "OWNER_TYPE_ID": 1,
                                    "TYPE_ID": 2,
                                    "OWNER_ID": result.result.result.add_lead,
                                    "COMMUNICATIONS": [{ 'VALUE': sPhone, 'TYPE': 'PHONE' }],
                                    "START_TIME": new Date().toISOString(),
                                    "END_TIME": new Date(Date.now() + 3600 * 1000).toISOString(),
                                    "SUBJECT": "Call back",
                                    "DESCRIPTION": "Call within an hour"
                                }
                            },
                            function(resultActivity) {
                                if (resultActivity.error()) {
                                    console.error(resultActivity.error());
                                }
                            }
                        );
                    }
                } else {
                    let errors = [];
                    if (result.result.result_error.add_lead) {
                        errors.push('error add lead: ' + result.result.result_error.add_lead.error_description);
                    }
                    if (result.result.result_error.get_lead) {
                        errors.push('error get new lead: ' + result.result.result_error.get_lead.error_description);
                    }
                    console.error(errors.join('\n'));
                }
            });
        });
    });
    ```

- PHP

    {% note info %}

    To use the examples in PHP, configure the *CRest* class and include the **crest.php** file in the files where this class is used. [More details](../../../how-to-use-examples.md)

    {% endnote %}

    ```php
    <?php
    $sName         = htmlspecialchars($_POST["NAME"]);
    $sLastName    = htmlspecialchars($_POST["LAST_NAME"]);
    $sPhone         = htmlspecialchars($_POST["PHONE"]);
    $sEmail         = htmlspecialchars($_POST["EMAIL"]);
        
    $arData = [
        'add_lead' => [
            'method' => 'crm.lead.add',
            'params' => [
                'fields'    => [
                    'TITLE' => 'From the site: ' . implode(' ', [$sName, $sLastName]),
                    'NAME' => (!empty($sName)) ? $sName : 'Empty name', // if simple mode crm NAME or LAST_NAME required for converting to contact
                    'LAST_NAME' => $sLastName,
                    'PHONE' => (!empty($sPhone)) ? array(array('VALUE' => $sPhone, 'VALUE_TYPE' => 'HOME')) : array(),
                    'EMAIL' => (!empty($sEmail)) ? array(array('VALUE' => $sEmail, 'VALUE_TYPE' => 'HOME')) : array()
                ]
            ]
        ],
        'get_lead' => [
            'method' => 'crm.lead.get',
            'params' => [
                'id' => '$result[add_lead]'
            ]
        ],
    ];
    $result = CRest::callBatch($arData);
        
    if(empty($result['result']['result_error']['add_lead']) && !empty($result['result']['result']['get_lead'])){
        // if status_id == converted on add then is simple mode crm
        if($result['result']['result']['get_lead']['STATUS_ID'] == 'CONVERTED'){
            $resultDeal = CRest::call('crm.deal.list',
                [
                    'filter'=>[
                        'LEAD_ID' => $result['result']['result']['add_lead']
                    ]
                ]);
            if(!empty($resultDeal['result']['0']['ID'])){
                $resultActivity = CRest::call('crm.activity.add', // call within an hour
                    [
                        'fields' =>[
                            "OWNER_TYPE_ID"     => 2, // 2 - is deal in CRest::call('crm.enum.ownertype');
                            "TYPE_ID"         => 2, // 2 - is call in CRest::call('crm.enum.activitytype');
                            "OWNER_ID"         => $resultDeal['result']['0']['ID'], // entity id
                            "COMMUNICATIONS"    => [['VALUE' => $sPhone,'TYPE' => 'PHONE']],
                            "START_TIME" => date("Y-m-d H:i:s",time()),
                            "END_TIME" => date("Y-m-d H:i:s",time()+3600),
                            "SUBJECT" => "Call back",
                            "DESCRIPTION" => "Call within an hour",
                        ]
                    ]);
            }
                
        } else {
            $resultActivity = CRest::call('crm.activity.add', // call within an hour
                [
                    'fields' =>[
                        "OWNER_TYPE_ID"     => 1, // 1 - is lead in CRest::call('crm.enum.ownertype');
                        "TYPE_ID"         => 2, // 2 - is call in CRest::call('crm.enum.activitytype');
                        "OWNER_ID"         => $result['result']['result']['add_lead'], // entity id
                        "COMMUNICATIONS"    => [['VALUE' => $sPhone,'TYPE' => 'PHONE']],
                        "START_TIME" => date("Y-m-d H:i:s",time()),
                        "END_TIME" => date("Y-m-d H:i:s",time()+3600),
                        "SUBJECT" => "Call back",
                        "DESCRIPTION" => "Call within an hour",
                    ]
                ]);
        }
            
    } else {
        if(!empty($result['result']['result_error']['add_lead']))
            $arResult[] = 'error add lead: '.$result['result']['result_error']['add_lead']['error_description'];
        if(!empty($result['result']['result_error']['get_lead']))
            $arResult[] = 'error get new lead: '.$result['result']['result_error']['get_lead']['error_description'];
    }    
    ?>
    ```

{% endlist %}