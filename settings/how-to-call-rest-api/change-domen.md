# Features of REST Calls When Changing the Bitrix24 Address

New cloud Bitrix24 accounts are created with generated addresses in the format `b24-xxx.bitrix24.yy`. Users can change this address at any time, subject to certain limitations. These limitations depend on the plan being used.

## Why This Is Important to Keep in Mind

If your application makes a REST call to Bitrix24 and uses a saved address on the application side, there may be a situation where this address is no longer valid.

When accessing an outdated address, Bitrix24 performs a redirect to the new one, but this redirect needs to be handled correctly in your code.

Most likely, if you are using GET parameters in your REST calls, you won't notice anything unusual, but with POST requests, it gets a bit more complicated.

In particular, if you are using PHP and curl, depending on the settings, a POST request during a redirect may "magically" turn into a GET request. In this case, the parameters sent in the POST request are simply lost.

{% note info %}

These features of working with the REST API are already taken into account in the [Bitrix SDK](../../first-steps/how-to-use-examples.md).

{% endnote %}

## Approach 1

When making a POST request, disable the redirect. If you receive a 302 status code, take the new address from the result and repeat the POST request, but to the new address.

{% list tabs %}

- Python

    ```python
    response = requests.post(url, allow_redirects=False)
    if response.status_code == 302:
        response = requests.post(response.headers['Location'])
    ```

- PHP

    ```php
    <?php

    $options = [
        'http' => [
            'method' => 'POST',
            'follow_location' => false
        ]
    ];

    $context = stream_context_create($options);
    $response = file_get_contents($url, false, $context);

    $headers = $http_response_header;
    $status_line = $headers[0];
    preg_match('{HTTP\/\S*\s(\d{3})}', $status_line, $match);
    $status_code = $match[1];

    if ($status_code == 302) {
        foreach ($headers as $header) {
            if (stripos($header, 'Location:') === 0) {
                $location = trim(substr($header, 9));
                $response = file_get_contents($location, false, $context);
                break;
            }
        }
    }

    ?>
    ```

{% endlist %}

## Approach 2

Use the option `curl_setopt($ch, CURLOPT_POSTREDIR, 3)`, which will allow you to handle the redirect situation.