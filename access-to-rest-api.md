# How to Access the REST API?

To use the REST API in Bitrix24, there are several important points to consider.

First, the REST API is only available on [paid plans](https://www.bitrix24.com/prices/) of Bitrix24, as well as during the demo plan. If you plan to automate for your specific Bitrix24, you need to switch to a paid plan.

Before purchasing a paid plan, you can test the capabilities of the REST API for 15 days by enabling the [demo plan](https://helpdesk.bitrix24.com/open/20237014/) on your account. To find out your account's current plan and whether the trial subscription is active, you can use the "My Plan" button in the header of the account.

[Technology partners](./market/technology-partnership.md) developing solutions for [listing in Bitrix24 Market](./market/index.md) can request a special NFR sandbox for the account where development is taking place. Upon approval of the request, Bitrix24 support will activate a special partner plan that supports the REST API on the specified account. The request for an NFR sandbox can be submitted through the chat in the Developer's area. This will allow you to use the REST API for developing and testing your solutions before publishing them in the Market.

Second, the capabilities of the REST API are limited by the permissions that the specific user making the requests has on the account, as any REST API call is made "on behalf of" that user. To have maximum access to the data and functionality of the account, it is advisable to use an administrator account. However, this does not mean that only the account administrator can use the REST API—any user has access to the REST API through the "Developer resources" section within Bitrix24. Just keep in mind that through the REST API, users can only access the functionality and data that is available to them through the user interface of the account.

Once you have confirmed that the REST API is available on your account, you can proceed to [make your first REST API call](./first-rest-api-call.md).