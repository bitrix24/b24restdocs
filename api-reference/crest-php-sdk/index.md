# Loading and Using CRest PHP SDK

Using the REST API allows for relatively straightforward solutions to many integration tasks with external sources or systems in Bitrix24, transferring data into Bitrix24, and much more. We have provided examples for the most common tasks that you can use as templates and modify to suit your needs.

All examples in this section are written using PHP, so you need to place this code on a server accessible to you while adhering to the following technical requirements:

1. The examples use the cURL module to perform REST requests. Learn [how to enable the cURL module](http://php.net/manual/en/book.curl.php) on your server.
2. A valid SSL certificate must be installed on your web server.
3. The examples use the basic SDK in the form of the *CRest* class to perform requests and extend authorization tokens. Take the [class code](https://github.com/bitrix-tools/crest), make the necessary changes related to authorization in the **settings.php** file, and place it on your server, inserting the example you need from the documentation.
4. If you encounter issues with the SDK, you can open the **checkserver.php** file in your browser, which will perform a minimal check of the server settings for the CRest class to function.

If the CRest class is used in the project and the encoding differs from UTF-8, two additional actions are required:

1. Open the files from the archive and change their encoding to the required one.
2. In the **settings.php** file, declare the constant `C_REST_CURRENT_ENCODING`. For example, if the project is in windows-1251 encoding, the constant should look like this: `define('C_REST_CURRENT_ENCODING','windows-1251');`

## Option 1. Calling REST Using Incoming [Webhook](../../local-integrations/local-webhooks.md)

1. Specify the webhook URL in the define `C_REST_WEB_HOOK_URL` in the **settings.php** file:

    ```php
    <?
    define('C_REST_WEB_HOOK_URL','https://xxx.bitrix24.com/rest/1/douasdqkjxgc3mgc1/');
    ```

2. Insert the example text into the **index.php** file:

    ```php
    <?
    require_once('crest.php');
    // put an example below
    echo '<PRE>';
    print_r(CRest::call(
        'crm.lead.add',
        [
            'fields' =>[
                'TITLE' => 'Lead Title', // Title*[string]
                'NAME' => 'First Name', // First Name[string]
                'LAST_NAME' => 'Last Name', // Last Name[string]
            ]
        ])
    );
    echo '</PRE>';
    ```

3. Enter the URL in the browser's address bar, for example, *https://mydomain.xxx/index.php* to see the result of the example.

## Option 2. Calling REST from a Local Application

1. In the **settings.php** file:

    ```php
    <?
    define('C_REST_CLIENT_ID','app.87816165165.35984727');
    define('C_REST_CLIENT_SECRET','g5dlcj3d2772h8g5jhzou907jojage');
    ```

2. Insert the example text into the **index.php** file:

    ```php
    <?
    require_once('crest.php');
    // put an example below
    echo '<PRE>';
    print_r(CRest::call(
        'crm.lead.add',
        [
            'fields' =>[
                'TITLE' => 'Lead Title', // Title*[string]
                'NAME' => 'First Name', // First Name[string]
                'LAST_NAME' => 'Last Name', // Last Name[string]
            ]
        ])
    );
    echo '</PRE>';
    ```

3. In the local application card, specify the URL of your application *https://mydomain.xxx/index.php* and the installation script URL *https://mydomain.xxx/install.php*.

4. Specify the values for the **client_id** and **client_secret** parameters for OAuth 2.0 authorization in the define `C_REST_CLIENT_ID` and `C_REST_CLIENT_SECRET` in the **settings.php** file, taking these values from the local application card.

    ```php
    <?
    require_once('crest.php');
    // put an example below
    echo '<PRE>';
    print_r(CRest::call(
        'crm.lead.add',
        [
            'fields' =>[
                'TITLE' => 'Lead Title', // Title*[string]
                'NAME' => 'First Name', // First Name[string]
                'LAST_NAME' => 'Last Name', // Last Name[string]
            ]
        ])
    );
    echo '</PRE>';
    ```

5. In the list of local applications, right-click on your local application and select "Reinstall." This is necessary for **install.php** to work correctly after you have inserted the correct values for `C_REST_CLIENT_ID` and `C_REST_CLIENT_SECRET`.

6. After installation, you will see the result of the example. If the example demonstrates embedding widgets into other Bitrix24 tools, you need to navigate to those tools.

## Option 3. Calling REST from a Mass-Market Application

1. In the **settings.php** file:

    ```php
    <?
    define('C_REST_CLIENT_ID','app.87816165165.35984727');
    define('C_REST_CLIENT_SECRET','g5dlcj3d2772h8g5jhzou907jojage');
    ```

2. Insert the example text into the **index.php** file:

    ```php
    <?
    require_once('crest.php');
    // put an example below
    echo '<PRE>';
    print_r(CRest::call(
        'crm.lead.add',
        [
            'fields' =>[
                'TITLE' => 'Lead Title', // Title*[string]
                'NAME' => 'First Name', // First Name[string]
                'LAST_NAME' => 'Last Name', // Last Name[string]
            ]
        ])
    );
    echo '</PRE>';
    ```

3. Add the mass-market application in the partner account to obtain `client_id` and `client_secret` and save the application.

4. Specify the values for the **client_id** and **client_secret** parameters for OAuth 2.0 authorization in the define `C_REST_CLIENT_ID` and `C_REST_CLIENT_SECRET` in the **settings.php** file.

    ```php
    <?
    require_once('crest.php');
    // put an example below
    echo '<PRE>';
    print_r(CRest::call(
        'crm.lead.add',
        [
            'fields' =>[
                'TITLE' => 'Lead Title', // Title*[string]
                'NAME' => 'First Name', // First Name[string]
                'LAST_NAME' => 'Last Name', // Last Name[string]
            ]
        ])
    );
    echo '</PRE>';
    ```

5. In the application card, add a version and specify the URL of your application *https://mydomain.xxx/index.php* and the installation script URL *https://mydomain.xxx/install.php* in the version card.

6. After saving the version, open the version card and click on the "Install on your Bitrix24" link to install your application on any available Bitrix24.

7. After installation, you will see the result of the example (if the example demonstrates embedding widgets into other Bitrix24 tools, you need to navigate to those tools).

8. For a real mass-market application, you need to inherit the CRest class, overriding the *getSettingData/setSettingData* methods, which handle obtaining/saving authorization tokens to a text file. These methods are not intended for operating the application on multiple Bitrix24 instances simultaneously.