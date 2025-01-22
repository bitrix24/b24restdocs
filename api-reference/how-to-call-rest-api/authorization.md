# Authorization in REST

The Bitrix24 REST API is an API that can be accessed by making HTTP requests to specific addresses of a particular Bitrix24 account. In fact, such requests can be made from any software that supports the HTTP protocol.

```bash
curl -X POST \
-H "Content-Type: application/json" \
-d '{
	"fields": {
		"title": "New Deal",
		"typeId": "SALE",
		"stageId": "NEW"
	},
	"auth": "YOUR_ACCESS_TOKEN"
}' \
https://your-domain.bitrix24.com/rest/crm.deal.add.json
```

In addition to the parameters of a specific method, it is necessary to pass some authorization data when making a request, without which the request will be considered unauthorized and blocked by Bitrix24.

Moreover, REST API methods are always called "on behalf of" a specific user of the Bitrix24 account. Essentially, the entire REST API is another way to use the existing functionality of Bitrix24, where the user directly accesses the product's functionality instead of using the user interface, calling various REST API methods.

There are two options for passing authorization data in requests to the REST API:

- Specifying a permanent incoming local webhook code;
- Specifying a temporary OAuth 2.0 authorization token, which is used in local and mass-market applications.

## Local Incoming Webhooks

Example of accessing the REST API using an incoming local webhook:

```bash
curl -X POST \
-H "Content-Type: application/json" \
-d '{
	"fields": {
		"title": "New Deal",
		"typeId": "SALE",
		"stageId": "NEW"
	}
}' \
https://your-domain.bitrix24.com/rest/1/8g9l071eismy9q2l/crm.deal.add.json
```

In this example, the following is specified:

- The address of a specific Bitrix24 account (`your-domain.bitrix24.com`);
- The user ID of the person who created the webhook (`1`);
- The secret code of the webhook (`8g9l071eismy9q2l`);
- The REST API method that adds a deal to the CRM (`crm.deal.add`).

The values of the method parameters (`fields`) were passed as a POST request.

In fact, "authorization" in this way of accessing the REST API is the user ID and the secret code of the webhook.

Webhooks are ideal for:

- Organizing one-time data import-export;
- Simple integrations with external and internal company systems (ERP, time tracking, auto-monitoring of software and hardware, etc.);
- Use in automating the processing of leads and deals (triggers and actions in CRM automation rules);

Using webhooks is simpler for technical implementation, as it does not require the implementation of the OAuth 2.0 protocol. Each user can add webhooks "for themselves," and the REST API called within such webhooks will be limited to the rights of the specific user-owner.

You can learn more about webhooks in the [corresponding lesson](https://helpdesk.bitrix24.com/courses/index.php?COURSE_ID=268&LESSON_ID=26002&LESSON_PATH=25400.25996.25998.26002) of our video course for developers.

## Local and Mass-Market Applications Using OAuth 2.0

Example of accessing the REST API using a temporary authorization token:

```bash
curl -X POST \
-H "Content-Type: application/json" \
-d '{
	"fields": {
		"title": "New Deal",
		"typeId": "SALE",
		"stageId": "NEW"
	},
	"auth": "807ca26600631fce00007a4b00000001f0f107255033363e91ab16442bd901b2571ed9"
}' \
https://your-domain.bitrix24.com/rest/crm.deal.add.json
```

In this example, the following is specified:

- The address of a specific Bitrix24 account (`your-domain.bitrix24.com`);
- The REST API method that adds a deal to the CRM (`crm.deal.add`);
- The authorization token specified in the `auth` parameter (`807ca26600631fce00007a4b00000001f0f107255033363e91ab16442bd901b2571ed9`).

The values of the method parameters (`fields`) were passed as a POST request.

"Authorization" in this way of accessing the REST API is the token specified in the `auth` parameter. It contains a number of important system values, including the ID of the specific Bitrix24 user on behalf of whom the REST API is being accessed.

You can learn how local and mass-market applications obtain authorization tokens in the [corresponding section](../oauth/index.md) of the documentation.

**Local applications**, unlike local webhooks, are better suited for tasks that require creating a user interface:

- Various reports;
- Additional auto-handlers within specific business logic;
- Solutions that require user access management;
- Chatbots and applications that extend the functionality of the Bitrix24 messenger;
- Additional operations for automated Bitrix24 workflows.

Local applications support all capabilities of the REST API and call REST using the OAuth 2.0 authorization protocol, but are installed only on a specific Bitrix24 account, without publication in the solutions catalog. Administrative access is required to add a local application.

**Mass-market solutions** are published in our [application catalog](../../market/index.md). They are available for installation to an unlimited number of users, and moreover, you can make them paid, selling their functionality on a subscription model.

Unlike the development of local applications for a specific project or client, where the relationship between the developer and the user is governed by their bilateral agreements, the publication of mass-market solutions in the Bitrix24 Marketplace catalog is regulated by Bitrix24 rules, which you will need to familiarize yourself with carefully.