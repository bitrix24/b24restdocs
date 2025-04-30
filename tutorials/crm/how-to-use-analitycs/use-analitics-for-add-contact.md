# How to Use Sales Intelligence When Creating a Deal and Contact

> Scope: [`crm`](../../../api-reference/scopes/permissions.md)
>
> Who can execute the method: users with administrative access to the CRM section

An example of using Sales Intelligence when creating a deal and contact. First, create a PHP page with a feedback web form: Full Name, Phone. Place the example code on the page.

What happens during the execution of the code?

1. The standard JS code from Bitrix24 Sales Intelligence is connected.
2. After filling out the form, in addition to the form fields, the code for Sales Intelligence `b24Tracker.guest.getTrace()` is passed in a hidden field.
3. A deal and a related contact are then created.
4. Finally, an analytics "trace" is registered for these objects, passing their types and identifiers in the following format:

```bash
/rest/crm.tracking.trace.add?ENTITIES[0][TYPE]=CONTACT&ENTITIES[0][ID]=3215&ENTITIES[1][TYPE]=LEAD&ENTITIES[1][ID]=1&TRACE=…
```

The Sales Intelligence script is installed on your site before the closing `</body>` tag on all pages of the site, including the page with the form.

{% list tabs %}

- JS

    ```javascript
    <!DOCTYPE html>
    <html lang="en">
    <head>
        <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/css/bootstrap.min.css" crossorigin="anonymous">
    </head>
    <body class="container">
    <h1>Feedback</h1>
    <div class="col-12">
        <p id="message"></p>
    </div>
    <form id="feedbackForm">
        <input type="hidden" id="FORM_TRACE" name="TRACE">
        <div class="row">
            <div class="col-4 mt-3">
                <label>Name*</label>
            </div>
            <div class="col-6 mt-3">
                <input type="text" name="NAME" required>
            </div>
        </div>
        <div class="row">
            <div class="col-4 mt-3">
                <label>Last name*</label>
            </div>
            <div class="col-6 mt-3">
                <input type="text" name="LAST_NAME" required>
            </div>
        </div>
        <div class="row">
            <div class="col-4 mt-3">
                <label>Phone*</label>
            </div>
            <div class="col-6 mt-3">
                <input type="text" name="PHONE" required>
            </div>
        </div>
        <div class="row">
            <div class="col-sm-10">
                <input type="submit" class="btn btn-primary" value="Send">
            </div>
        </div>
    </form>
    <script>
        document.getElementById('feedbackForm').addEventListener('submit', function(event) {
            event.preventDefault();
            var formData = new FormData(event.target);
            var fields = {
                NAME: formData.get('NAME'),
                LAST_NAME: formData.get('LAST_NAME'),
                PHONE: [{ value: formData.get('PHONE') }]
            };

            BX24.callMethod(
                'crm.contact.add',
                { fields: fields },
                function(resultContact) {
                    if (resultContact.error()) {
                        document.getElementById('message').innerText = 'Feedback has not been saved: ' + resultContact.error_description();
                    } else {
                        var arDealFields = {
                            TITLE: 'Feedback page: ' + formData.get('NAME') + ' ' + formData.get('LAST_NAME'),
                            CONTACT_ID: resultContact.data()
                        };

                        BX24.callMethod(
                            'crm.deal.add',
                            { fields: arDealFields },
                            function(resultDeal) {
                                if (resultDeal.error()) {
                                    document.getElementById('message').innerText = 'Feedback has not been saved: ' + resultDeal.error_description();
                                } else {
                                    if (formData.get('TRACE')) {
                                        BX24.callMethod(
                                            'crm.tracking.trace.add',
                                            {
                                                ENTITIES: [
                                                    { TYPE: 'CONTACT', ID: resultContact.data() },
                                                    { TYPE: 'DEAL', ID: resultDeal.data() }
                                                ],
                                                TRACE: formData.get('TRACE')
                                            },
                                            function(resultTrace) {
                                                if (resultTrace.error()) {
                                                    document.getElementById('message').innerText = 'Feedback has not been saved: ' + resultTrace.error_description();
                                                } else {
                                                    document.getElementById('message').innerText = 'Feedback saved';
                                                }
                                            }
                                        );
                                    } else {
                                        document.getElementById('message').innerText = 'Feedback saved';
                                    }
                                }
                            }
                        );
                    }
                }
            );
        });

        window.onload = function(e) {
            var traceDom = document.getElementById('FORM_TRACE');
            if (traceDom) {
                var trace = b24Tracker.guest.getTrace();
                traceDom.value = trace;
            }
        }
    </script>
    </body>
    </html>
    ```

- PHP

    {% note info %}

    To use the examples in PHP, configure the *CRest* class and include the **crest.php** file in the files where this class is used. [More details](../../../how-to-use-examples.md)

    {% endnote %}

    ```php
    <!DOCTYPE html>
    <html lang="en">
    <head>
        <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/css/bootstrap.min.css" crossorigin="anonymous">
    </head>
    <body class="container">
    <h1>Feedback</h1>
    <?php
    include("crest.php");
    $message = '';
    if(!empty($_POST['SAVE']))
    {
        $fields = [
            'NAME' => $_POST['NAME'],
            'LAST_NAME' => $_POST['LAST_NAME'],
            'PHONE' => [ [ 'value'=>$_POST['PHONE'] ] ],
        ];
        $resultContact = CRest::call(
            'crm.contact.add',
            [
                'fields' => $fields
            ]
        );
        if (!empty($resultContact['result']))
        {
            $arDealFields = [
                'TITLE' => 'Feedback page: '.$_POST['NAME'].' '.$_POST['LAST_NAME'],
                'CONTACT_ID' => $resultContact['result']
            ];
            $resultDeal = CRest::call(
                'crm.deal.add',
                [
                    'fields' => $arDealFields
                ]
            );
            if (!empty($resultDeal['result']))
            {
                if(!empty($_POST['TRACE']))
                {
                    $resultTrace = CRest::call(
                        'crm.tracking.trace.add',
                        [
                            'ENTITIES' => [
                                [
                                    'TYPE' => 'CONTACT',//COMPANY, CONTACT, DEAL, LEAD, QUOTE
                                    'ID' => $resultContact['result']
                                ],
                                [
                                    'TYPE' => 'DEAL',//COMPANY, CONTACT, DEAL, LEAD, QUOTE
                                    'ID' => $resultDeal['result']
                                ]
                            ],
                            'TRACE' =>    $_POST['TRACE']
                        ]
                    );
                }
                $message = 'Feedback saved';
            }
            elseif (!empty($resultDeal['error_description']))
            {
                $message =    'Feedback has not been saved: '.$resultDeal['error_description'];
            }
            else
            {
                $message = 'Feedback has not been saved';
            }
        }
        elseif (!empty($resultContact['error_description']))
        {
            $message =    'Feedback has not been saved: '. $resultContact['error_description'];
        }
        else
        {
            $message = 'Feedback has not been saved';
        }
    }
    ?>
    <div class="col-12">
        <p><?=$message?></p>
    </div>
    <form method="post" action="">
        <input type="hidden" id="FORM_TRACE" name="TRACE">
        <div class="row">
            <div class="col-4 mt-3">
                <label>Name*</label>
            </div>
            <div class="col-6 mt-3">
                <input type="text" name="NAME" required>
            </div>
        </div>
        <div class="row">
            <div class="col-4 mt-3">
                <label>Last name*</label>
            </div>
            <div class="col-6 mt-3">
                <input type="text" name="LAST_NAME" required>
            </div>
        </div>
        <div class="row">
            <div class="col-4 mt-3">
                <label>Phone*</label>
            </div>
            <div class="col-6 mt-3">
                <input type="text" name="PHONE" required>
            </div>
        </div>
        <div class="row">
            <div class="col-sm-10">
                <input type="submit" name="SAVE" class="btn btn-primary" value="Send">
            </div>
        </div>
    </form>
    <script>
        window.onload = function(e){
            var traceDom = document.getElementById('FORM_TRACE');
            if(traceDom)
            {
                var trace = b24Tracker.guest.getTrace();
                traceDom.value = trace;
            }
        }
    </script>
    </body>
    </html>
    ```

{% endlist %}