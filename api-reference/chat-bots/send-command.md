# How to Send Commands and Extend the Authorization Key

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- edits needed to meet writing standards

{% endnote %}

{% endif %}

## Interaction Using PHP

In our course, we interact with Bitrix24 using **PHP**.

All examples are provided using the **restCommand** function:

```php
restCommand(
    'imbot.message.add',
    Array(
        "DIALOG_ID" => $_REQUEST['data']['PARAMS']['DIALOG_ID'],
        "MESSAGE" => "Enter search string",
    ),
    $_REQUEST["auth"]
);
```

where:
- **The first parameter** is the API name (`imbot.message.add`);
- **The second parameter** is the data sent to the API (`Array(...)`);
- **The third parameter** is the authorization data for the request (`$_REQUEST["auth"]`).

### The `restCommand` Function

The `restCommand` function sends a REST request to Bitrix24:

{% note info %}

This function is used as an example. You can use your own function based on the [interaction protocol](../how-to-call-rest-api/general-principles.md), or the javascript method [BX24.callMethod](../bx24-js-sdk/how-to-call-rest-methods/bx24-call-method.md), the tiny and simple [CRest SDK](../crest-php-sdk/index.md) or industry standard [B24PhpSDK](https://github.com/bitrix24/b24phpsdk).

{% endnote %}

## The restCommand Function

```php
/**
* Send rest query to Bitrix24.
*
* @param $method - Rest method, ex: methods
* @param array $params - Method params, ex: Array()
* @param array $auth - Authorize data, ex: Array('domain' => 'https://test.bitrix24.com', 'access_token' => '7inpwszbuu8vnwr5jmabqa467rqur7u6')
* @param boolean $authRefresh - If authorization is expired, refresh token
* @return mixed
*/
function restCommand($method, array $params = Array(), array $auth = Array(), $authRefresh = true)
{
$queryUrl = "https://".$auth["domain"]."/rest/".$method;
$queryData = http_build_query(array_merge($params, array("auth" => $auth["access_token"])));

$curl = curl_init();

curl_setopt_array($curl, array(
     CURLOPT_POST => 1,
     CURLOPT_HEADER => 0,
     CURLOPT_RETURNTRANSFER => 1,
     CURLOPT_SSL_VERIFYPEER => 1,
     CURLOPT_URL => $queryUrl,
     CURLOPT_POSTFIELDS => $queryData,
));

$result = curl_exec($curl);
curl_close($curl);

$result = json_decode($result, 1);

if ($authRefresh && isset($result['error']) && in_array($result['error'], array('expired_token', 'invalid_token')))
{
     $auth = restAuth($auth);
     if ($auth)
     {
         $result = restCommand($method, $params, $auth, false);
     }
}

return $result;
}
```

## The restAuth Function

```php
/**
* Get new authorization data if your authorization has expired.
*
* @param array $auth - Authorize data, ex: Array('domain' => 'https://test.bitrix24.com', 'access_token' => '7inpwszbuu8vnwr5jmabqa467rqur7u6')
* @return bool|mixed
*/
function restAuth($auth)
{
if (!CLIENT_ID || !CLIENT_SECRET)
     return false;

if(!isset($auth['refresh_token']) || !isset($auth['scope']) || !isset($auth['domain']))
     return false;

$queryUrl = 'https://'.$auth['domain'].'/oauth/token/';
$queryData = http_build_query($queryParams = array(
     'grant_type' => 'refresh_token',
     'client_id' => CLIENT_ID,
     'client_secret' => CLIENT_SECRET,
     'refresh_token' => $auth['refresh_token'],
     'scope' => $auth['scope'],
));

$curl = curl_init();

curl_setopt_array($curl, array(
     CURLOPT_HEADER => 0,
     CURLOPT_RETURNTRANSFER => 1,
     CURLOPT_URL => $queryUrl.'?'.$queryData,
));

$result = curl_exec($curl);
curl_close($curl);

$result = json_decode($result, 1);

return $result;
}
```