# Security Recommendations for Applications Using REST API

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not to be deployed on prod_" %}

- Tokens received must be verified on the authorization server.
- The application_token in events must be checked.
- Webhooks should not be used for authorization within the frontend.
- Do not store unnecessary personal information on your server.
- Access permissions to file sections should be separated if there are multiple applications on the server. Databases of applications and users with access to each application should be separated.

{% endnote %}

{% endif %}