# Server-side Local Application with User Interface

The application utilizes a simplified version of OAuth 2.0, displayed as an additional page that shows the full name of the current user. The archive of the example consists of [SDK CRest](https://github.com/bitrix-tools/crest/), [modification of SDK CRest](../api-reference/crest-php-sdk/using-in-users-context.md) for the simplified use of OAuth 2.0, and a PHP file index.php containing the example code. You need to place the files from the example archive on your web server before installing it in Bitrix24.

> **Attention!** This example operates based on *SDK CRest*. Before using the example, you must open the file **checkserver.php** in your browser and verify the correctness of your server settings. [Learn more](../how-to-use-examples.md).

[Download the archive](https://bitrixsoft.com/docs/marketplace-and-apps24/local-server-ui-index.zip)

You can install the local application either from the **Developer resources** section (*Applications > Developer resources, tab "Ready-made scenarios" > Other > Local application*), or by following this path: Applications (1) — Developer resources (2) — Other (3) — Local application (4):

![Adding application](./_images/local_add_sm.jpg)

![](./_images/local_add_4.jpg)

In the opened form, fill in the basic fields and specify the necessary permissions for the application (for our example, user management permissions are required), indicating the **Path to your handler** (this means that your application must already be physically accessible via URL over HTTPS before you add it to your Bitrix24).

![Application addition form](./_images/server-ui-local-form_1-new.png)

After saving, the new application will be displayed in the list of integrations (*Applications > Developer resources > Integrations*) in your Bitrix24.

![List of integrations](./_images/server-ui-local-added_new.png)

Find the application **Full Name** in the left menu or in the **More** menu under Applications and launch it.

The launched application will output debug information about the authorization data of the current user, as well as the full name of the current user, retrieving it via REST API using this authorization data.

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

![Launched application](./_images/server-ui-local-runned.jpg)

{% endnote %}

{% endif %}

Since this application operates within the Bitrix24 interface and uses the authorization of the current user who opened the application, it functions solely within the permissions of that user.

## Continue Exploring

- [{#T}](static-local-app.md)
- [{#T}](serverside-local-app-with-no-ui.md)