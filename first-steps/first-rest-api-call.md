# How to Make Your First API Request?

Log into your Bitrix24 account and navigate to the Developer resources section via the link in the left menu. To see this menu item, you need to expand the Applications section.

## Incoming Webhook

In the opened slider, click on the "Other" section, and then select the "Incoming Webhook" block.

{% note info "Incoming Webhook" %}

The simplest way to use REST is through what is known as an _incoming webhook_, which is a permanent "key" API with the permissions of the user who created this webhook.

{% endnote %}

When you open the incoming webhook creation slider, you will see that Bitrix24 has already generated a unique code for this webhook.

{% note alert "Keep the code secret!" %}

This code is essentially an access key or password for the webhook, so you must ensure that it remains confidential.

{% endnote %}

## Request Generator

Below the webhook code in the slider, you will see the **Request Generator** block.

It allows you to make various requests to the Bitrix24 REST API directly from the Bitrix24 interface by selecting different methods from the list and configuring the corresponding method parameters. You can find information about specific parameters in the documentation by following the links provided.

Once you have specified the parameters and their values, Bitrix24 will generate the complete HTTP request for you. You can preview it, copy it to an external system (or browser) for execution, or you can execute it right here and now by clicking the **Execute** button.

When you execute the request, you will receive a response from Bitrix24 in JSON format. Congratulations on your first successful call to the Bitrix24 REST API!

{% note info %}

Incoming webhooks are a very convenient tool for using the REST API to solve individual tasks. However, if you want to implement more complex scenarios in your Bitrix24 that involve [different users](../local-integrations/local-apps.md), or if you want to develop a [mass-market solution](../market/index.md) for placement in the Bitrix24 Marketplace, you will need to use [OAuth 2.0 authorization](../settings/oauth/index.md) with temporary secure authorization tokens for working with REST.

{% endnote %}