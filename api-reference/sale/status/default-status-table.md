# Default Status Table

By default, the system creates a set of statuses. Some of these are system statuses, and there are certain restrictions on working with them— for example, system statuses cannot be deleted. Any restrictions (if they exist) are described in the specific [methods for working with statuses](./index.md).

#|
|| **Status Identifier** | **Type**  | **Description** | **System** ||
|| `N` | Order | Accepted, awaiting payment | Yes ||
|| `S` | Order | Shipped | No ||
|| `P` | Order | Paid, preparing for shipment | No ||
|| `D` | Order | Canceled | No ||
|| `F` | Order | Completed | Yes ||
|| `DN` | Delivery | Awaiting processing | Yes ||
|| `DD` | Delivery | Canceled | No ||
|| `DF` | Delivery | Dispatched | Yes ||
|#

## Continue Your Exploration

- [{#T}](./index.md)
- [{#T}](./sale-status-add.md)
- [{#T}](./sale-status-update.md)
- [{#T}](./sale-status-get.md)
- [{#T}](./sale-status-list.md)
- [{#T}](./sale-status-delete.md)
- [{#T}](./sale-status-get-fields.md)