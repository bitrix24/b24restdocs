# Get the list of countries documentgenerator.region.list

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- examples are missing
- response in case of error is missing

{% endnote %}

{% endif %}

> Scope: [`documentgenerator`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `documentgenerator.region.list` returns a list of regions, both default and custom.

No parameters.

## Successful response

> 200 OK

```json
"regions": {
	"uk":{ // pre-installed region
		"code": "uk",
		"title": "United Kingdom",
		"languageId": "en",
	},
	"1":{ // custom region
		"id": "1",
		"title": "Turkey asdf",
		"languageId":" ",
		"formatDate": "YYYY-MM-DD",
		"formatDatetime": "YYYY-MM-DD HH:MI:SS",
		"formatName": "#LAST_NAME# #NAME# #SECOND_NAME#",
	}
}
```