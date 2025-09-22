# How to Use Sales Intelligence When Creating a Lead

> Scope: [`crm`](../../../api-reference/scopes/permissions.md)
>
> Who can execute the method: users with administrative access to the CRM section

An example of using Sales Intelligence when creating a lead. First, create a PHP page with a feedback form: First Name, Last Name, Phone. Place the example code on the page.

What happens during the execution of the code?

1. The standard JS code from Bitrix24's Sales Intelligence is connected.
2. After filling out the form, in addition to the form fields, a hidden field transmits the code for Sales Intelligence `b24Tracker.guest.getTrace()`.
3. Then, the method [crm.lead.add](../../../api-reference/crm/leads/crm-lead-add.md) is called, where the code from `getTrace` is added to the `TRACE` field.

The Sales Intelligence script is installed on your site before the closing `</body>` tag on all pages of the site, including the page with the form.

{% list tabs %}

- JS

    ```js
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
                        <label>First Name*</label>
                    </div>
                    <div class="col-6 mt-3">
                        <input type="text" name="NAME" required>
                    </div>
                </div>
                <div class="row">
                    <div class="col-4 mt-3">
                        <label>Last Name*</label>
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
                    var traceInput = document.getElementById('FORM_TRACE');
                    if(traceInput)
                    {
                        traceInput.value = b24Tracker.guest.getTrace();
                    }
                }

                document.getElementById('feedbackForm').addEventListener('submit', function(event) {
                    event.preventDefault();
                    var formData = new FormData(event.target);
                    var fields = {
                        TRACE: formData.get('TRACE'),
                        NAME: formData.get('NAME'),
                        LAST_NAME: formData.get('LAST_NAME'),
                        PHONE: [{ value: formData.get('PHONE') }]
                    };

                    BX24.callMethod(
                        'crm.lead.add',
                        { fields: fields },
                        function(result) {
                            var messageElement = document.getElementById('message');
                            if(result.error()) {
                                messageElement.textContent = 'Feedback has not been saved: ' + result.error_description();
                            } else {
                                messageElement.textContent = 'Feedback saved';
                            }
                        }
                    );
                });
            </script>
        </body>
    </html>
    ```

- PHP

    {% note info %}

    To use the examples in PHP, configure the *CRest* class and include the **crest.php** file in the files where this class is used. [More details](../../../first-steps/how-to-use-examples.md)

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
                    'TRACE' => $_POST['TRACE'],
                    'NAME' => $_POST['NAME'],
                    'LAST_NAME' => $_POST['LAST_NAME'],
                    'PHONE' => [ [ 'value'=>$_POST['PHONE'] ] ],
                ];
                $result = CRest::call(
                    'crm.lead.add',
                    [
                        'fields' => $fields
                    ]
                );
                if (!empty($result['result']))
                {
                    $message = 'Feedback saved';
                }
                elseif (!empty($result['error_description']))
                {
                    $message =    'Feedback has not been saved: '.$result['error_description'];
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
                        <label>First Name*</label>
                    </div>
                    <div class="col-6 mt-3">
                        <input type="text" name="NAME" required>
                    </div>
                </div>
                <div class="row">
                    <div class="col-4 mt-3">
                        <label>Last Name*</label>
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
                    var traceInput = document.getElementById('FORM_TRACE');
                    if(traceInput)
                    {
                        traceInput.value = b24Tracker.guest.getTrace();
                    }
                }
            </script>
        </body>
    </html>
    ```

{% endlist %}