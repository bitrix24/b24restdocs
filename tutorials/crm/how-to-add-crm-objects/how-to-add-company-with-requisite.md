# Add a Company with Requisites

> Scope: [`crm`](../../../api-reference/scopes/permissions.md)
>
> Who can execute the method: users with administrative access to the CRM section

Example of embedding a form on a webpage, after filling out which a new company is created in Bitrix24 with attached requisites.

- Create a form on the desired page:

{% list tabs %}

- JS

    ```js
    BX24.callMethod('crm.address.fields', {}, function(arAddressFields) {
        BX24.callMethod('crm.requisite.preset.list', {
            'select': ["ID", "NAME"]
        }, function(arRequisiteType) {
            if (arRequisiteType.error()) {
                console.error(arRequisiteType.error() + ': ' + arRequisiteType.error_description());
            } else {
                if (arRequisiteType.data().length > 0) {
                    var requisiteTypeOptions = arRequisiteType.data().map(function(item) {
                        return '<option value="' + item.ID + '">' + item.NAME + '</option>';
                    }).join('');

                    var addressFields = arAddressFields.data();
                    delete addressFields.TYPE_ID;
                    delete addressFields.ENTITY_TYPE_ID;
                    delete addressFields.ENTITY_ID;
                    delete addressFields.COUNTRY_CODE;
                    delete addressFields.ANCHOR_TYPE_ID;
                    delete addressFields.ANCHOR_ID;

                    var addressFieldsInputs = Object.keys(addressFields).map(function(key) {
                        var field = addressFields[key];
                        return '<input type="text" name="ADDRESS[' + key + ']" placeholder="' + field.title + '" ' + (field.isRequired ? 'required' : '') + '>';
                    }).join('');

                    var formHtml = `
                        <form id="form_to_crm">
                            <select name="REQ_TYPE" required>
                                <option value="" disabled selected>Select</option>
                                ${requisiteTypeOptions}
                            </select>
                            <input type="text" name="TITLE" placeholder="Org name" required>
                            <input type="text" name="INN" placeholder="INN">
                            <input type="text" name="PHONE" placeholder="Phone">
                            ${addressFieldsInputs}
                            <input type="submit" value="Submit">
                        </form>
                    `;

                    document.body.innerHTML = formHtml;

                    document.getElementById('form_to_crm').addEventListener('submit', function(el) {
                        el.preventDefault();
                        var formData = new FormData(this);
                        fetch('form.php', {
                            method: 'POST',
                            body: formData
                        })
                        .then(response => response.json())
                        .then(data => {
                            alert(data.message);
                        })
                        .catch(error => {
                            console.error('Error:', error);
                        });
                    });
                } else {
                    document.body.innerHTML = 'No requisite types.';
                }
            }
        });
    });
    ```

- PHP

    {% note info %}

    To use the PHP examples, configure the *CRest* class and include the **crest.php** file in the files where this class is used. [Learn more](../../../how-to-use-examples.md)

    {% endnote %}

    ```php
    <?php
    $arAddressFields = CRest::call('crm.address.fields',[]);
    /*
    crm.address.fields return: /api-reference/crm/requisites/addresses/index.html
    */
    $arRequisiteType = CRest::call('crm.requisite.preset.list',
    [
        'select'=>[
            "ID", "NAME"
        ]
    ]);
    if(!empty($arRequisiteType['result'])):
        $arRequisiteType = array_column($arRequisiteType['result'],'NAME','ID');
        //unset system address fields
        unset($arAddressFields['result']['TYPE_ID']);
        unset($arAddressFields['result']['ENTITY_TYPE_ID']);
        unset($arAddressFields['result']['ENTITY_ID']);
        //unset uninteresting address fields
        unset($arAddressFields['result']['COUNTRY_CODE']);
        unset($arAddressFields['result']['ANCHOR_TYPE_ID']);
        unset($arAddressFields['result']['ANCHOR_ID']);
        ?>
        <form id="form_to_crm">
            <select name="REQ_TYPE" required>
                <option value="" disabled selected>Select</option>
                <?php foreach($arRequisiteType as $id=>$name):?>
                    <option value="<?=$id?>"><?=$name?></option>
                <?php endforeach;?>
            </select>
            <input type="text" name="TITLE" placeholder="Org name" required>
            <input type="text" name="INN" placeholder="INN">
            <input type="text" name="PHONE" placeholder="Phone">
            <?php if(is_array($arAddressFields['result'])):?>
                <?php foreach($arAddressFields['result'] as $key=>$arField):?>
                    <input type="text" name="ADDRESS[<?=$key?>]" placeholder="<?=$arField['title']?>" <?=($arField['isRequired'])?'required':'';?>>
                <?php endforeach;?>
            <?php endif;?>
            <input type="submit" value="Submit">
        </form>
    <?php else:?>
        <?='No requisite types.';?>
    <?php endif;?>
    <script src="https://ajax.googleapis.com/ajax/libs/jquery/3.3.1/jquery.min.js"></script>
    <script>
        $(document).ready(function() {
            $('#form_to_crm').on( 'submit', function(el) {//event submit form
                el.preventDefault();//the default action of the event will not be triggered
                var formData = $(this).serialize();
                $.ajax({
                    'method': 'POST',
                    'dataType': 'json',
                    'url': 'form.php',
                    'data': formData,
                    success: function(data){//success callback
                        alert(data.message);
                    }
                });
            });
        });
    </script>
    ```

{% endlist %}

- Create a file **form.php** to save the filled forms:

{% list tabs %}

- JS

    ```javascript
    var iRequisitePresetID = parseInt(BX24.placement.call('getValue', 'REQ_TYPE'));
    var sTitle = BX24.placement.call('getValue', 'TITLE');
    var sINN = BX24.placement.call('getValue', 'INN');
    var sPhone = BX24.placement.call('getValue', 'PHONE');
    var arAddress = {};

    document.querySelectorAll('[name^="ADDRESS"]').forEach(function(input) {
        var key = input.name.match(/\[([^\]]+)\]/)[1];
        arAddress[key] = input.value;
    });

    arAddress['TYPE_ID'] = 1; // 1 is actual address in BX24.callMethod('crm.enum.addresstype');
    arAddress['ENTITY_TYPE_ID'] = 8; // 8 - is requisite in BX24.callMethod('crm.enum.ownertype');

    var arPhone = sPhone ? [{ 'VALUE': sPhone, 'VALUE_TYPE': 'WORK' }] : [];

    BX24.callMethod(
        'crm.company.add',
        {
            'fields': {
                'TITLE': sTitle,
                'COMPANY_TYPE': 'CUSTOMER', // is Client in BX24.callMethod('crm.status.list', { 'filter': { 'ENTITY_ID': 'COMPANY_TYPE' } })
                'PHONE': arPhone,
            }
        },
        function(result) {
            if (result.error()) {
                console.error(result.error() + ': ' + result.error_description());
                console.log(JSON.stringify({ 'message': 'Company not added: ' + result.error_description() }));
            } else {
                var companyId = result.data();
                BX24.callMethod(
                    'crm.requisite.add',
                    {
                        'fields': {
                            'ENTITY_TYPE_ID': 4, // 4 - is company in BX24.callMethod('crm.enum.ownertype');
                            'ENTITY_ID': companyId, // company id
                            'PRESET_ID': iRequisitePresetID,
                            'TITLE': sTitle,
                            'ACTIVE': 'Y',
                            'NAME': sTitle,
                            'RQ_INN': sINN,
                        }
                    },
                    function(resultRequisite) {
                        if (resultRequisite.error()) {
                            console.error(resultRequisite.error() + ': ' + resultRequisite.error_description());
                        } else {
                            arAddress['ENTITY_ID'] = resultRequisite.data(); // id requisite
                            BX24.callMethod(
                                'crm.address.add',
                                {
                                    'fields': arAddress
                                },
                                function(resultAddress) {
                                    if (resultAddress.error()) {
                                        console.error(resultAddress.error() + ': ' + resultAddress.error_description());
                                    } else {
                                        console.log(JSON.stringify({ 'message': 'Company added' }));
                                    }
                                }
                            );
                        }
                    }
                );
            }
        }
    );
    ```

- PHP

    {% note info %}

    To use the PHP examples, configure the *CRest* class and include the **crest.php** file in the files where this class is used. [Learn more](../../../how-to-use-examples.md)

    {% endnote %}

    ```php
    <?php
    $iRequisitePresetID = intVal($_POST["REQ_TYPE"]);
    $sTitle = htmlspecialchars($_POST["TITLE"]);
    $sINN = htmlspecialchars($_POST["INN"]);
    $sPhone = htmlspecialchars($_POST["PHONE"]);
    $arAddress = [];

    foreach($_POST["ADDRESS"] as $key=>$val){
        $arAddress[$key] = htmlspecialchars($val);
    }
    $arAddress['TYPE_ID'] = 1;//1 is actual address in CRest::call('crm.enum.addresstype');
    $arAddress['ENTITY_TYPE_ID'] = 8;//8 - is requisite in CRest::call('crm.enum.ownertype');

    $arPhone = (!empty($sPhone)) ? array(array('VALUE' => $sPhone, 'VALUE_TYPE' => 'WORK')) : array();

    $result = CRest::call(
        'crm.company.add',
        [
            'fields' =>[
                'TITLE' => $sTitle,
                'COMPANY_TYPE' => 'CUSTOMER',//is Client in CRest::call('crm.status.list',['filter'=>['ENTITY_ID'=>'COMPANY_TYPE']])
                'PHONE' => $arPhone,
            ]
        ]
    );
    if(!empty($result['result'])){
        $resultRequisite = CRest::call(
            'crm.requisite.add',
            [
                'fields' =>[
                    'ENTITY_TYPE_ID' => 4,//4 - is company in CRest::call('crm.enum.ownertype');
                    'ENTITY_ID' => $result['result'],//company id
                    'PRESET_ID' => $iRequisitePresetID,
                    'TITLE' => $sTitle,
                    'ACTIVE' => 'Y',
                    'NAME' => $sTitle,
                    'RQ_INN' => $sINN,
                ]
            ]
        );
        if(!empty($resultRequisite['result'])){
            $arAddress['ENTITY_ID'] = $resultRequisite['result'];//id requisite
            $resultAddress = CRest::call(
                'crm.address.add',
                [
                    'fields' =>$arAddress
                ]
            );
        }
        echo json_encode(['message' => 'Company added']);
    }elseif(!empty($result['error_description'])){
        echo json_encode(['message' => 'Company not added: '.$result['error_description']]);
    }else{
        echo json_encode(['message' => 'Company not added']);
    }
    ?>
    ```

{% endlist %}