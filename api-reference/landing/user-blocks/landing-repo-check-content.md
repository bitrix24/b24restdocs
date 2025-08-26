# Check Content for Dangerous Substrings landing.repo.checkContent

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- parameter types are not specified
- parameter requirements are not indicated
- examples are missing
- no response in case of error

{% endnote %}

{% endif %}

> Scope: [`landing`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `landing.repo.checkContent` checks the content for dangerous substrings. These include `onclick=""`, `<iframe>`, and several others. In typical use cases, the chances of triggering are minimal. The method is used solely for content control during [block registration](./landing-repo-register.md).

## Parameters

#|
|| **Parameter** | **Description** | **Available since** ||
|| **content**
[`unknown`](../../data-types.md) | Content to be tested. ||
|| **splitter**
[`unknown`](../../data-types.md) | Optional parameter for separating dangerous substrings. Defaults to `#SANITIZE#`. ||
|#

## Examples

{% list tabs %}

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'landing.repo.checkContent',
    		{
    			content: '<div style="color: red" onclick="alert(123)"><iframe src="//evil.com"></iframe></div>',
    			splitter: '#AAA#'
    		}
    	);
    	
    	const result = response.getData().result;
    	if(result.error())
    		console.error(result.error());
    	else
    		console.info(result);
    }
    catch(error)
    {
    	console.error('Error:', error);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'landing.repo.checkContent',
                [
                    'content'  => '<div style="color: red" onclick="alert(123)"><iframe src="//evil.com"></iframe></div>',
                    'splitter' => '#AAA#',
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        if ($result->error()) {
            error_log($result->error());
        } else {
            echo 'Info: ' . print_r($result->data(), true);
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error checking content: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'landing.repo.checkContent',
        {
            content: '<div style="color: red" onclick="alert(123)"><iframe src="//evil.com"></iframe></div>',
            splitter: '#AAA#'
        },
        function(result)
        {
            if(result.error())
                console.error(result.error());
            else
                console.info(result.data());
        }
    );
    ```

{% endlist %}

{% include [Examples note](../../../_includes/examples.md) %}

# Response in case of success

> 200 OK
```json
content:"<div style="color: red" oncl#AAA#ick="alert(123)"><ifr#AAA#ame src="//evil.com"></iframe></div>"
is_bad:true
```

Essentially, the label `is_bad = true` indicates that there are dangerous areas in the content, along with the text marked by the separators in those dangerous areas. The developer should modify such areas before registration.