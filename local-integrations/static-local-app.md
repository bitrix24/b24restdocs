# Static Local Application

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- Is a note needed?

{% endnote %}

{% endif %}

> **Attention!** The static local application and its deployment described below are intended for cloud Bitrix24 accounts.
> 
> This type can be used in on-premise accounts if:
> - the application is uploaded to any folder in the file structure of the on-premise account
> - the handler specifies the path to this folder

The archive with the example contains one file and represents a ready-made application that accesses the REST API and displays the full name of the current user.

[Download archive](https://helpdesk.bitrix24.com/examples/index.html.zip)

You can install the local application either from the **Developer resources** section (*Applications > Developer resources, tab "Ready-made scenarios" > Other > Local application*), or by following this path: Applications (1) — Developer resources (2) — Other (3) — Local application (4):



In the opened form, fill in the basic fields, upload the archive, and specify the necessary permissions (for our example, user management permissions are required):


After saving, the new application will be displayed in the integrations list (*Applications > Developer resources > Integrations*) in your Bitrix24.


Find **Full Name** in the left menu or in the **More** menu in the Applications section and launch it. The launched application will display the full name of the current user, retrieving it via the REST API through the JS library. Since the static local application operates within the Bitrix24 interface, the JS SDK automatically obtains and uses the authorization of the current user who opened the application, and acts solely within the permissions of that user.

## Continue Exploring

- [{#T}](serverside-local-app-with-ui.md)
- [{#T}](serverside-local-app-with-no-ui.md)