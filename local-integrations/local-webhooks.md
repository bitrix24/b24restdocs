# Inbound and Outbound Webhooks

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- Add to "Typical use-cases and scenarios" about ready scenarios for webhooks.

{% endnote %}

{% endif %}

A local webhook is a simplified way to access the methods and events of the Bitrix24 REST API, specifically designed for use within a single account.

Local webhooks are divided into inbound and outbound.

## Inbound Webhook

Through the user interface of the Developer resources section, you can obtain and record the authorization key — the inbound webhook.

This key can then be used to call REST methods, and it does not have an expiration date (unlike OAuth 2.0 authorization tokens).

This makes webhooks an exceptionally simple and convenient mechanism for working with REST, but it is important to understand that this simplicity has its drawbacks:

- User participation is required to create a webhook (you cannot generate webhooks automatically).
- Since the webhook does not expire, any "leak" of the webhook URL can lead to unauthorized access to your Bitrix24 within the permissions of that webhook. For this reason, this mechanism is suitable for "internal" integrations but not for mass-market use cases.
- A number of REST methods are not available for use through webhooks, as their logic requires the "context" of an application, and there is no application in terms of Bitrix24 for webhooks (in particular, methods for embedding applications in the Bitrix24 interface, telephony events, some chatbot events, etc.).

Despite these limitations, for the vast majority of integration tasks within a specific project, webhooks are an ideal option for working with the REST API.

{% note tip "Typical use-cases and scenarios" %}

- [{#T}](../tutorials/crm/index.md)
- [{#T}](../tutorials/ai/add-joke-prompt.md)

{% endnote %}

You can create an inbound webhook from the **Developer resources** section (*Applications > Developer resources, tab "Ready Scenarios" > Other > Inbound Webhook*).

In the opened form:

- Change the name of the webhook.
- In the request generator, select the REST API method (you can read the method description and download a ready code example with the necessary parameters for making requests).
- Test the webhook by clicking the **Execute** button.
- Specify access permissions, allowing only certain Bitrix24 tools to make requests.

The request generator will provide a sample URL that should be used when sending data from an external system to Bitrix24.

![Sample URL](./_images/dev_url.png)

**The URL consists of:**

- **doc-test-b24.bitrix24.com** — your Bitrix24 address.
- **/rest** — indicating that the work is being done through REST with webhooks.
- **/1** — the identifier of the user who created the webhook.
- **/173glortu42lvpju** — the secret code.

> **Attention!** This code is confidential information. It must be kept secret.

- **/crm.contact.get** — the called REST API method. In this case, it is the method that returns a contact by its identifier.
- **.json** — an optional parameter ("transport"). When creating new webhooks, you can omit this (by default, `.json` will be used). In the ready solutions constructor, `.json` is explicitly included.
- **?ID=42** — parameters required for the specific method. In this case, it is the identifier. Parameters are specified after the question mark and separated by the `&` symbol.

## Outbound Webhook

For some scenarios, it would be convenient for our automation to trigger automatically when a user changes some data in Bitrix24. For this, there is a tool in local integrations called "Outbound Webhook."

{% note tip "Typical use-cases and scenarios" %}

- [{#T}](../tutorials/crm/index.md)
- [{#T}](../tutorials/ai/add-joke-prompt.md)

{% endnote %}

> **Attention!** An active license is required for the outbound webhook to work in the on-premise version of Bitrix24; it will not work on demo accounts.

You can create an outbound webhook from the **Developer resources** section (*Applications > Developer resources, tab Ready Scenarios > Other > Outbound Webhook*).

In the opened form:

1. Change the name of the webhook.
2. Specify **the URL of your handler** — the page on an external resource where the webhook will send requests.
3. Select the event that will trigger the webhook.

   When creating an outbound webhook, a token will be generated in the form of a string of random characters. This code will allow you to verify within the handler that the handler was indeed called by your Bitrix24.

   ![Outbound Webhook](./_images/webhook_add.png)

4. Place the following code on the handler page:

   Example handler code for the event [ONCRMDEALUPDATE](../api-reference/crm/deals/events/on-crm-deal-update.md)

   ```php
   <?
   print_r($_REQUEST);
   writeToLog($_REQUEST, 'inbound');
   /**
   * Write data to log file.
   *
   * @param mixed $data
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
       file_put_contents(getcwd() . '/hook.log', $log, FILE_APPEND);
       return true;
   }
   ```

   To test, open any deal for editing and save changes; the log will display a history similar to this:

   ```plaintext
   2017.01.17 12:58:29
   inbound
   Array
   (
       [event] => ONCRMDEALUPDATE
       [data] => Array
           (
               [FIELDS] => Array
                   (
                       [ID] => 662
                   )

           )

       [ts] => xxx
       [auth] => Array
           (
               [domain] => xxx.bitrix24.com
               [client_endpoint] => https://xxx.bitrix24.com/rest/
               [server_endpoint] => https://oauth.bitrix.info/rest/
               [member_id] => xxx
               [application_token] => xxx
           )

   )
   ```

## Continue Learning

- [{#T}](local-apps.md)