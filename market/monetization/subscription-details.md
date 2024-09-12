# What is a Subscription Model?

**Bitrix24 Market Plus Subscription** is a way to provide end customers access to a whole set of applications for Bitrix24 at a fixed cost, as well as a method for calculating royalties to distribute Bitrix's revenue among individual developers.

To ensure fair distribution, we divide the revenue generated from the subscription sales of each individual customer among those developers whose solutions were actually used on that specific Bitrix24. This allows authors to earn even from solutions targeted at a narrow audience.

## Revenue Distribution Principle

We help solution developers earn through Bitrix24 Market. To achieve this, we create conditions that assist developers in creating quality solutions: we direct payments to those developers whose solutions prompted customers to purchase the subscription, and to those developers whose solutions users ultimately utilized within the subscription.

**85% OF REVENUE GOES TO DEVELOPERS!**

1C-Bitrix retains a small portion—only 15%. Developers of solutions participating in the subscription receive 85% of the revenue!

**50% REWARD AT THE TIME OF PURCHASE**

Half of the reward is immediately allocated to the applications that led to the subscription sale. We consider the actual usage of such applications to distribute the revenue among them in the correct proportions. This helps solution developers receive monetary payments from new clients.

**50% BASED ON USAGE**

The second half of the reward is distributed throughout the subscription's duration among the applications that the client uses. This helps solution developers receive stable monetary payments from a growing number of regular users.

## Reward for Purchasing a Subscription

How do we determine which applications customers purchase the subscription for? Our system tracks which applications are used and distributes the reward among them. There are several scenarios:

### USAGE DURING TRIAL

A customer may first decide to try the subscription trial, install several applications, remove some, and ultimately find those applications that are useful enough for them to become a Market Plus subscription buyer. Among those solutions that were installed, used during the subscription trial, and not removed at the time of subscription purchase, we will distribute 50% of the revenue immediately after payment in proportions proportional to usage.

### USAGE AFTER PURCHASE

A customer may purchase a subscription without a prior trial, believing that the solutions included in the subscription will surely be useful. They then start installing and evaluating applications after the subscription payment. Among those solutions that were installed, used from the time of purchase until the end of the reporting month, and not removed by the end of the reporting month, we will distribute 50% of the revenue at the beginning of the next month in proportions proportional to usage.

### USAGE BEFORE RENEWAL

If the applications continue to be useful for the customer, they will renew their Market Plus subscription. In this case, we check which applications were used by the customer during the 30 calendar days prior to the renewal payment and were not removed by the end of the reporting month, and we distribute 50% of the revenue among them immediately after payment in proportions proportional to usage.

## Reward for Application Usage

It is important not only which applications attracted customers to the subscription but also which solutions customers continue to use in the end. Our system records daily which applications are used on each individual Bitrix24 and distributes 50% of the reward among them.

## How are Shares Distributed Among Applications?

Every day on each Bitrix24 where a customer purchased a subscription to Bitrix24 Market, we allocate points for the usage of each solution. Each day, these points are summed up, and the shares of the subscription cost corresponding to one day of usage are distributed among developers (as are the shares from the reward for purchasing the subscription). Ultimately, these amounts accumulate and are transferred to the developers.

By **usage**, we mean not just the installation of a solution on Bitrix24, but the users' interaction with the functionality of that solution:

1. REST API calls;
2. User navigation to the solution's interface (including widget integrations);
3. Triggering REST event handlers;
4. Triggering workflow actions, automation rules, and triggers;
5. Presence of published stores and websites created using templates or blocks from solutions;
6. Presence of installed configuration solutions (ready-made CRMs, smart scenarios, etc.);

When publishing a solution in the Market catalog, moderators specify the **complexity category (weight)** for each solution from a predefined list, and the monitoring system automatically determines the **frequency (mode) of usage** as the application operates on each individual Bitrix24.

Every day, each solution distributed under the subscription earns usage points for each Bitrix24 where it is used, based on a simple formula: **solution weight * usage frequency coefficient**.

### Weight/Complexity

Different solutions have different "weights" when earning points, depending on the functionality offered by each solution. We have tried to make these categories clear and fair so that they ultimately correspond to both the complexity of development and the benefit to the business. The moderator records the category when reviewing the moderation request for the solution and indicates the category with the highest weight if the solution fits multiple categories:

| Application Type | Weight       | Description    |
| ---------------- | ------------ | --------------- |
| 1                | 10           | Multifunctional diverse interface within Bitrix24, at least 2 types of integrations (the settings interface for user options is not counted as user functionality) |
| 2                | 8            | Multifunctional diverse interface within Bitrix24, less than 2 types of integrations (the settings interface for user options is not counted as user functionality) |
| 3                | 8            | Bitrix24 online store template                                     |
| 4                | 5            | Triggers, automation rules, workflow actions                            |
| 5                | 5            | Auto-processor for REST events                                            |
| 6                | 5            | AI provider for Bitrix24 Copilot                                     |
| 7                | 5            | Bitrix24 website template                                                 |
| 8                | 5            | Dashboard for BI constructor                                            |
| 9                | 5            | Open lines connector                                               |
| 10               | 5            | Integration with telephony or SMS provider                            |
| 11               | 5            | Solutions for mass mailings or integration with mailing services       |
| 12               | 5            | Chatbot builders or solutions with ready-made chatbots for Bitrix24 |
| 13               | 5            | Mass data export/import                                         |
| 14               | 3            | Payment systems or delivery services                                  |
| 15               | 3            | Custom field types                                            |
| 16               | 1            | Industry-specific CRMs                                                         |
| 17               | 1            | Bitrix24 group/project templates (planned)                       |
| 18               | 1            | Bitrix24 knowledge base templates (planned)                           |
| 19               | 1            | Smart scenarios                                                         |
| 20               | 1            | Prompt for Bitrix24 Copilot                                           |
| 21               | 1            | Other                                                                 |

### Usage Frequency

Solutions can benefit businesses daily or periodically. There are scenarios (usually related to obtaining analytical reports) that require user interaction once a week or even once a month. To balance the subscription model for both daily mass-use solutions and products with periodic scenarios, we automatically account for the mode of interaction with the application and apply a correction coefficient in the points allocation.

A solution is considered daily on a specific Bitrix24 if used more than two days a week; weekly if used more than once a month; or monthly if used no more than once a month. If a solution has not been accessed for more than a month, point allocation stops, even if the solution technically remains installed on Bitrix24.

Unlike the weight assigned by the moderator to the solution as a whole, the usage mode is automatically determined for each solution on each individual Bitrix24. This means that on one Bitrix24, your solution may be used daily, while on another, it may be used once a month with the corresponding correction coefficient. Points are allocated daily in this case.

| Coefficient  | Usage Mode       | Examples of Solutions |
| ------------ | ----------------- | --------------------- |
| 1            | Daily             | automation on events or automation rules, interface integrations, chatbots, open lines connectors, industry-specific CRMs, smart scenarios, published websites, etc. |
| 0.7          | Weekly            | operational analytics, data export/import, lead generation, mailings, etc. |
| 0.5          | Monthly           | reporting analytics, data export/import, lead generation, etc. |

## Example of Reward Calculation for Usage

The easiest way to understand everything is through a specific example. Let's take one specific Bitrix24 where a user installed two different solutions from two authors and assume that our system collected information about their usage. For simplicity, let’s say both solutions are daily-use solutions with a correction coefficient of 1.

**Solution 1-1** from the first developer is a ready-made online store for Bitrix24 (type 3 with a weight of 8), while **Solution 2-1** is a workflow action (type 4 with a weight of 5).

From May 15, 2019, to May 17, 2019, both solutions were used on the account xxx.bitrix24.com, and on May 18, 2019, only **Solution 2-1** was used.

In this case, the reward calculation will occur as follows (the total daily reward is indicated for example purposes; in reality, it is calculated based on the specific subscription cost depending on the Bitrix24 tariff plan, the presence of any marketing discounts, and sales through the partner network):

| Date        | Account          | Developer      | Software     | Type | Points for Usage | Total Daily Reward | Total Points for the Day | Points for Solution Usage | Solution Usage Coefficient | Daily Reward for Licensor |
| ----------- | ---------------- | -------------- | ------------ | ---- | ---------------- | ------------------ | ------------------------ | ------------------------- | -------------------------- | ------------------------- |
| 05/15/2019  | xxx.bitrix24.com | Developer-1    | Solution 1-1 | 3    | 8                | 9.9167             | 13                       | 8                         | 0.6154                   | 6.1026                   |
| 05/15/2019  | xxx.bitrix24.com | Developer-2    | Solution 2-1 | 4    | 5                | 9.9167             | 13                       | 5                         | 0.3846                   | 3.8141                   |
| 05/16/2019  | xxx.bitrix24.com | Developer-1    | Solution 1-1 | 3    | 8                | 9.9167             | 13                       | 8                         | 0.6154                   | 6.1026                   |
| 05/16/2019  | xxx.bitrix24.com | Developer-2    | Solution 2-1 | 4    | 5                | 9.9167             | 13                       | 5                         | 0.3846                   | 3.8141                   |
| 05/17/2019  | xxx.bitrix24.com | Developer-1    | Solution 1-1 | 3    | 8                | 9.9167             | 13                       | 8                         | 0.6154                   | 6.1026                   |
| 05/17/2019  | xxx.bitrix24.com | Developer-2    | Solution 2-1 | 4    | 5                | 9.9167             | 13                       | 5                         | 0.3846                   | 3.8141                   |
| 05/18/2019  | xxx.bitrix24.com | Developer-2    | Solution 2-1 | 4    | 5                | 9.9167             | 5                        | 5                         | 1.00                     | 9.9167                   |

## Why Does This Work?

This example clearly demonstrates several important things about revenue distribution:

1. The more different Bitrix24 accounts use your solution, the more you earn.
2. The larger your share in the daily revenue distribution, the more you earn.
3. The more customers used your solution during the subscription trial before payment, the more you earn.
4. If a customer pays for a subscription but does not use your solutions, you do not participate in the revenue distribution from them.
5. If a customer pays for a subscription and uses only your solutions, you receive **all 85% of the revenue**.

Developers always claim the same share of revenue from the subscription cost. The model only affects how that share is distributed among specific developers.

We chose this model because it is the best way to distribute revenue, and for success, you need to adhere to several principles:

1. Create solutions that are needed by the widest possible audience.
2. Develop unique solutions that fully address the needs of niche businesses.
3. Avoid trying to copy solutions that are already widely used by a large number of clients.
4. Do not attempt to game the system; create truly quality and useful solutions.

The subscription sales model does not limit you in choosing a product strategy: success can come from creating super hits "for everyone" or from developing unique multifunctional niche solutions.