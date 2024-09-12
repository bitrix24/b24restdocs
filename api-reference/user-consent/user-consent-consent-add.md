# Save the User Consent userconsent.consent.add

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- parameter types are not specified
- parameter requirements are not indicated
- examples are missing
- success response is absent
- error response is absent

{% endnote %}

{% endif %}

> Scope: [`userconsent`](../scopes/permissions.md)
>
> Who can execute the method: any user

The method `userconsent.consent.add` saves the user's consent.

#|
|| **Parameter** | **Description** ||
|| **agreement_id** | Agreement code. ||
|| **ip** | User's IP address. ||
|| **url** | The page where consent was obtained. ||
|| **origin_id** | Source, for example, **my_contact_form**. ||
|| **originator_id** | Unique code within the source, for example, e-mail. ||
|#