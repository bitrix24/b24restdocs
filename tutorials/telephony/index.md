# Main User Scenarios and Example

{% note warning "We are still updating this page" %}

Some data may be missing — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for writing standards

{% endnote %}

{% endif %}

1. An incoming call to the PBX on a specific user's internal number should display the call card for the employee in Bitrix24 (Method [telephony.externalcall.register](../../api-reference/telephony/telephony-external-call-register.md)).

2. An incoming call from an unknown number (not registered in CRM) should enter the processing queue (a list of users who should answer incoming calls):
   - Simultaneous queue: all employees who are not currently answering other calls will simultaneously see the call card, and when one of them starts answering the call, the card disappears for the others (first `telephony.externalcall.register` for the first in the queue, then [telephony.externalcall.show](../../api-reference/telephony/telephony-external-call-show.md) for the others).
   - Sequential queue: each employee in the queue who is not currently answering other calls will see the call card for a certain period (3-5-7 seconds); if the employee does not start answering the call, the card disappears, and the call is transferred to the next in the queue (first `telephony.externalcall.register` for the first in the queue, then [telephony.externalcall.hide](../../api-reference/telephony/telephony-external-call-hide.md) and `telephony.externalcall.show` for the next).

3. An incoming call from a known number is displayed as a call card in Bitrix24 for the manager responsible for the corresponding CRM object. (First `telephony.externalcall.register` with SHOW = 0, which will return either CREATED_LEAD if the phone was not found in CRM and a new lead was created, or a pair of CRM_ENTITY_TYPE and CRM_ENTITY_ID indicating the found existing client. Simultaneously, CRM_ACTIVITY_ID is returned with the identifier of the new CRM activity in which the call will be recorded later. Knowing the identifier of the object in CRM, one can use REST methods to get the identifier of the manager responsible for the client, transfer the call to them, and show them the call card `telephony.externalcall.show`).

4. An employee in Bitrix24 clicks on the phone number in the CRM interface. The application initiates an outgoing call to the specified number on the PBX side (event `onexternalcallstart`, the employee sees the call card `telephony.externalcall.register`).

5. The call is completed (incoming or outgoing). The fact of the call and the recording are recorded in connection with the CRM object ([telephony.externalcall.finish](../../api-reference/telephony/telephony-external-call-finish.md)). If at the time of the call's completion the recording of the conversation is not yet ready on the PBX, then instead of `telephony.externalcall.finish`, we first simply hide the call card `telephony.externalcall.hide`, and then, when the recording is ready, we call `telephony.externalcall.finish`.

6. An incoming call occurred on the PBX at a time when there is no connection between the PBX and Bitrix24 for some reason. After the connection is restored, information about the occurred call is recorded in Bitrix24 (see 1-3, but without showing the call card – sequentially calling the methods `telephony.externalcall.register` with the parameter SHOW = 0 and `telephony.externalcall.finish`).

{% note info %}

To add the recording to the call in the calling scenario, applications must pass CALL_LIST_ID that they receive in the [event](../../api-reference/telephony/events/on-external-call-start.md) at the start of the call.

{% endnote %}

## Example

```php
<?php
/**
 * Created by PhpStorm.
 * User: John
 * Date: 01.11.16
 * Time: 10:44
 */
// ini_set('display_errors','Off');
// forming the URL of our script for use in AJAX requests from the application interface
$script_url = ($_SERVER['SERVER_PORT'] == 443 ? 'https' : 'http') . '://' . $_SERVER['SERVER_NAME'] . (in_array($_SERVER['SERVER_PORT'],
    array(80, 443)) ? '' : ':' . $_SERVER['SERVER_PORT']) . $_SERVER['SCRIPT_NAME'];
$appsConfig = array();
$b24domain = $_REQUEST['DOMAIN'];
// if we received an outgoing call event, then authorization is passed through the auth node in the request array
// but we only need the domain from there, we have already saved the authorization by this time
if (!empty($_REQUEST['auth'])) {
    $b24domain = $_REQUEST['auth']['domain'];
}
$configFileName = '/config_' . trim(str_replace('.', '_', $b24domain)) . '.php';
echo getcwd().$configFileName."<br/>";
if (file_exists(getcwd() . $configFileName)) {
    include_once getcwd() . $configFileName;
} else {
    // saving tokens of the user setting up the application
    $appsConfig = $_REQUEST;
    saveParams($appsConfig);
    // registering the outgoing call event
    restCommand('event.bind', array(
        "event" => 'ONEXTERNALCALLSTART',
        "handler" => $script_url."?action=outcoming",
    ),
    $b24domain, $appsConfig['AUTH_ID']);
    /* test event to check the mechanism
    restCommand('event.bind', array(
        "event" => 'ONAPPTEST',
        "handler" => $script_url."?action=test",
        ),
        $b24domain, $appsConfig['AUTH_ID']);
    */
}
$action = $_REQUEST['action'];
// we just launched the application in the Bitrix24 interface
if ($action == '') {
?>
<html>
<head>
    <meta charset="utf-8"><meta http-equiv="X-UA-Compatible" content="IE=edge">
<script src="https://ajax.googleapis.com/ajax/libs/jquery/3.1.1/jquery.min.js"></script>
<!-- Latest compiled and minified CSS -->
<link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap.min.css" integrity="sha384-BVYiiSIFeK1dGmJRAkycuHAHRg32OmUcww7on3RYdg4Va+PmSTsz/K68vbdEjh4u" crossorigin="anonymous">
<!-- Optional theme -->
<link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap-theme.min.css" integrity="sha384-rHyoN1iRsVXV4nD0JutlnGaslCJuC7uwjduW9SVrLvRYooPp2bWYgmgJQIXwl/Sp" crossorigin="anonymous">
<!-- Latest compiled and minified JavaScript -->
<script src="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/js/bootstrap.min.js" integrity="sha384-Tc5IQib027qvyjSMfHjOMaLkfuWVxZxUPnCJA7l2mCWNIpG9mGCD8wGNIcPD7Txa" crossorigin="anonymous"></script>
    <script src="//api.bitrix24.com/api/v1/"></script>
</head>
<body>
<div class="form-group">
    <label for="IncomingNumber">Incoming call number</label>
    <input type="text" class="form-control" id="incomingNumber" placeholder="phone">
</div>
<div class="form-group">
    <label for="user1">User 1</label>
    <input type="text" class="form-control" id="user1" placeholder="user id" value="1">
</div>
<div class="form-group">
    <label for="user2">User 2 (for call transfer)</label>
    <input type="text" class="form-control" id="user2" placeholder="user id">
</div>
<a class="btn btn-default" href="#" role="button" id="incoming">Incoming</a>
<a class="btn btn-default" href="#" role="button" id="redirect">Redirect</a>
<a class="btn btn-default" href="#" role="button" id="drop">Drop</a>
<a class="btn btn-default" href="#" role="button" id="test">Event test</a>
<div id="debug"></div>
<?
    // if you're curious, you can see what authorization parameters Bitrix24 passes to the application script
    // when the application is executed in a frame inside Bitrix24
    //echo "<pre>";
    //print_r($_REQUEST);
    //echo "</pre>";
?>
<script>
    $( "#incoming" ).on( "click", function( event ) {
        // here we simulate the operation of an external PBX, in particular, receiving an incoming call
        // hence AJAX, passing authorization parameters, etc.
        // in real practice, REST telephony will be called from the PBX side, and there we have already saved
        // the authorization tokens (see $appsConfig$appsConfig) and we know which Bitrix24 to send
        // the REST call to, which users to show the card to, etc.
        auth = BX24.getAuth();
        $.ajax({
            url: "<?=$script_url?>",
            data: {
                action: 'incoming',
                user1: $( "#user1" ).val(),
                phone: $( "#incomingNumber" ).val(),
                DOMAIN: auth['domain']
            },
            success: function( result ) {
                $( "#debug" ).html( result );
            }
        });
    });
    $( "#redirect" ).on( "click", function( event ) {
        auth = BX24.getAuth();
        $.ajax({
            url: "<?=$script_url?>",
            data: {
                action: 'redirect',
                user1: $( "#user1" ).val(),
                user2: $( "#user2" ).val(),
                DOMAIN: auth['domain']
            },
            success: function( result ) {
                $( "#debug" ).html( result );
            }
        });
    });
    $( "#drop" ).on( "click", function( event ) {
        auth = BX24.getAuth();
        $.ajax({
            url: "<?=$script_url?>",
            data: {
                action: 'drop',
                user1: $( "#user1" ).val(),
                user2: $( "#user2" ).val(),
                DOMAIN: auth['domain']
            },
            success: function( result ) {
                $( "#debug" ).html( result );
            }
        });
    });
    /* initiating a test event on the server-side script, nothing important
    $( "#test" ).on( "click", function( event ) {
        auth = BX24.getAuth();
        $.ajax({
            url: "<?=$script_url?>",
            data: {
                action: 'eventtest',
                DOMAIN: auth['domain']
            },
            success: function( result ) {
                $( "#debug" ).html( result );
            }
        });
    });
    */
</script>
</body>
</html>
<? } else {
    switch ($action) {
        case 'test': writeToLog(array('test' => $_REQUEST), 'telephony test event');
                break;
        case 'outcoming':
            writeToLog(array('outcoming' => $_REQUEST), 'telephony event');
            $result = restCommand('telephony.externalCall.register',
                array(
                    "USER_ID" => $_REQUEST['data']['USER_ID'],
                    "PHONE_NUMBER"   => $_REQUEST['data']['PHONE_NUMBER'],
                    "TYPE" => '1',
                    "CRM_CREATE" => 1
                ),
                $b24domain, $appsConfig['AUTH_ID']);
            $appsConfig['CALL'] = $result['result'];
            saveParams($appsConfig);
            break;
        case 'eventtest':
            writeToLog(array('eventtest' => $_REQUEST), 'test event call');
            $result = restCommand('event.test',
                array(
                ),
                $b24domain, $appsConfig['AUTH_ID']);
            echo "test event call";
            break;
        case 'incoming':
            $result = restCommand('telephony.externalCall.register',
                array(
                    "USER_ID" => $_REQUEST['user1'],
                    "PHONE_NUMBER"   => $_REQUEST['phone'],
                    "TYPE" => '2',
                    "CRM_CREATE" => true
                ),
                $b24domain, $appsConfig['AUTH_ID']);
            $appsConfig['CALL'] = $result['result'];
            saveParams($appsConfig);
            echo "incoming <pre>";
            print_r($appsConfig);
            echo "</pre>";
            break;
        case 'redirect':
            echo "redirect <pre>";
            print_r($appsConfig);
            echo "</pre>";
            if ($appsConfig['CALL']['CALL_ID'] != '') {
                $result = restCommand('telephony.externalCall.hide',
                    array(
                        "CALL_ID" => $appsConfig['CALL']['CALL_ID'],
                        "USER_ID" => $_REQUEST['user1']
                    ),
                    $b24domain, $appsConfig['AUTH_ID']);
                $result = restCommand('telephony.externalCall.show',
                    array(
                        "CALL_ID" => $appsConfig['CALL']['CALL_ID'],
                        "USER_ID" => $_REQUEST['user2']
                    ),
                    $b24domain, $appsConfig['AUTH_ID']);
            }
            echo "redirected to ".$_REQUEST['user2'];
            break;
        case 'drop':
            writeToLog(array('config' => $appsConfig), 'call is finishing');
            if ($appsConfig['CALL']['CALL_ID'] != '') {
                $result = restCommand('telephony.externalCall.finish',
                    array(
                        "CALL_ID" => $appsConfig['CALL']['CALL_ID'],
                        "USER_ID" => $_REQUEST['user1'],
                        "DURATION"   => '120',
                        "STATUS_CODE" => '200',
                        "ADD_TO_CHAT" => true
                    ),
                    $b24domain, $appsConfig['AUTH_ID']);
                $appsConfig['CALL'] = $result['result'];
                saveParams($appsConfig);
                echo "finished <pre>";
                print_r($appsConfig);
                echo "</pre>";
                writeToLog(array('request' => $_REQUEST, 'config' => $appsConfig), 'call is finished');
            }
            echo "dropped and saved";
        break;
    }
}
/**
 * Save application configuration.
 *
 * @param $params
 *
 * @return bool
 */
function saveParams($params) {
    $config = "<?php\n";
    $config .= "\$appsConfig = " . var_export($params, true) . ";\n";
    $config .= "?>";
    $configFileName = '/config_' . trim(str_replace('.', '_', $_REQUEST['DOMAIN'])) . '.php';
    file_put_contents(getcwd() . $configFileName, $config);
    return true;
}
/**
 * Send rest query to Bitrix24.
 *
 * @param	   $method - Rest method, ex: methods
 * @param array $params - Method params, ex: array()
 * @param array $auth   - Authorize data, ex: array('domain' => 'https://test.bitrix24.com', 'access_token' => '7inpwszbuu8vnwr5jmabqa467rqur7u6')
 *
 * @return mixed
 */
function restCommand($method, array $params = array(), $auth_domain, $access_token) {
    $queryUrl  = 'https://' . $auth_domain . '/rest/' . $method;
    $queryData = http_build_query(array_merge($params, array('auth' => $access_token)));
    writeToLog(array('URL' => $queryUrl, 'PARAMS' => array_merge($params, array("auth" => $access_token))), 'telephony send data');
    $curl = curl_init();
    curl_setopt_array($curl, array(
        CURLOPT_SSL_VERIFYPEER => 0,
        CURLOPT_POST		   => 1,
        CURLOPT_HEADER		 => 0,
        CURLOPT_RETURNTRANSFER => 1,
        CURLOPT_URL			=> $queryUrl,
        CURLOPT_POSTFIELDS	 => $queryData,
        CURLOPT_VERBOSE		 => 1
    ));
    $result = curl_exec($curl);
    writeToLog(array('raw' => $result), 'telephony got data');
    curl_close($curl);
    $result = json_decode($result, 1);
    return $result;
}
/**
 * Write data to log file.
 *
 * @param mixed  $data
 * @param string $title
 *
 * @return bool
 */
function writeToLog($data, $title = '') {
    $log = "\n------------------------\n";
    $log .= date("Y.m.d G:i:s") . "\n";
    $log .= (strlen($title) > 0 ? $title : 'DEBUG') . "\n";
    $log .= print_r($data, 1);
    $log .= "\n------------------------\n";
    file_put_contents(getcwd() . '/tel.log', $log, FILE_APPEND);
    return true;
}
?>
```

{% include [Note on Examples](../../_includes/examples.md) %}