# Incoming Event Queue

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

An incoming queue helps receive [REST API events](../../api-reference/events/index.md) and outgoing webhooks without overloading the handler. The handler verifies the request source, stores the data in a queue, and responds to Bitrix24. Separate workers perform resource-intensive business logic asynchronously. This approach complements the general [performance recommendations](./index.md).

## Basic Principle

1. **Requests arrive at the server.** When Bitrix24 sends an HTTP request, it reaches the server or load balancer. Verify the request source using `auth.application_token`:
   - for an application event, compare it with the token retained by the application during installation
   - for an outgoing webhook, compare it with the value of the *Application token* field in the webhook settings

   For details, see [Security in Event Handlers](../../api-reference/events/safe-event-handlers.md). Outgoing webhook setup is described in [Incoming and Outgoing Webhooks](../../local-integrations/local-webhooks.md).
2. **Requests are added to the queue.** Store the request data in a database or message queue. Return a successful HTTP response only after the data has been stored reliably. Perform resource-intensive business logic asynchronously after responding.
3. **Requests are retrieved from the queue.** One or more workers retrieve and process requests sequentially or in parallel, depending on available resources.

{% note warning "" %}

Bitrix24 does not resend an event if the handler does not respond or returns an error HTTP status. Do not return a successful response until the request has been stored in the queue.

{% endnote %}

The internal queue may deliver a request again after a worker failure. Make processing idempotent so that a retry does not modify data twice.

## Advantages

1. **Load management.** Queues process requests sequentially and prevent server overload during traffic spikes.
2. **Improved stability.** A reliable queue retains requests during high load and allows processing to be retried after a failure.
3. **Scalability.** Queues allow the system to scale by adding more workers or servers.
4. **Fast Bitrix24 response.** The handler acknowledges the request immediately after storing it and does not wait for business logic to finish.

## Implementation Options

Choose an approach based on reliability, replay, and operational requirements. You can use a database, Redis, or a message broker.

### 1. Store in a Database

A database is suitable if you do not need separate queue infrastructure. The handler stores the request body and metadata in a table. A worker selects records with the `pending` status, processes them, and updates their status. Under high load, contention for the table may reduce performance.

In these PHP examples, `$requestData` contains validated incoming request data, and `$db` is a configured PDO connection. Adapt the table structure and exception handling to your project.

{% list tabs %}

- Event Handler

    ```php
    // Store the request before returning a successful HTTP response
    $statement = $db->prepare(
        'INSERT INTO requests (data, status) VALUES (:data, :status)'
    );
    $statement->execute([
        'data' => json_encode($requestData, JSON_THROW_ON_ERROR),
        'status' => 'pending',
    ]);
    ```

- Worker

    ```php
    // Reserve one request so another worker cannot take it at the same time
    $db->beginTransaction();
    $request = $db->query(
        "SELECT * FROM requests WHERE status = 'pending' " .
        "ORDER BY id LIMIT 1 FOR UPDATE SKIP LOCKED"
    )->fetch();

    if ($request) {
        $statement = $db->prepare(
            'UPDATE requests ' .
            'SET status = :status, processing_started_at = CURRENT_TIMESTAMP ' .
            'WHERE id = :id'
        );
        $statement->execute([
            'status' => 'processing',
            'id' => $request['id'],
        ]);
    }
    $db->commit();

    if ($request) {
        processRequest(json_decode($request['data'], true));
        $statement = $db->prepare(
            'UPDATE requests ' .
            'SET status = :status, processing_started_at = NULL ' .
            'WHERE id = :id'
        );
        $statement->execute([
            'status' => 'processed',
            'id' => $request['id'],
        ]);
    }
    ```

{% endlist %}

`FOR UPDATE SKIP LOCKED` requires database management system support. Add a `processing_started_at` field to the table. If a request remains in `processing` longer than allowed, a separate background task must return it to `pending`.

### 2. Use Redis

Redis is suitable as a fast queue under high load.

- incoming requests are added using `LPUSH`
- workers use `BRPOPLPUSH` to atomically move requests to a processing list. After successful processing, the request is removed from that list

In the example, `$redis` is a configured Redis connection, and `$requestData` contains validated incoming request data.

{% list tabs %}

- Event Handler

    ```php
    // Store the request in Redis before returning a successful HTTP response
    $redis->lPush('request_queue', json_encode($requestData));
    ```

- Worker

    ```php
    // Atomically move a message to the processing list
    while ($request = $redis->brPopLPush(
        'request_queue',
        'request_processing',
        5
    )) {
        processRequest(json_decode($request, true));
        $redis->lRem('request_processing', $request, 1);
    }
    ```

{% endlist %}

If the worker stops before removing the request from `request_processing`, return the stalled request to the main queue using a separate background task. Store the time it entered processing separately so the task does not return a request that is still running.

### 3. RabbitMQ or Kafka

RabbitMQ and Kafka require separate infrastructure but provide built-in delivery and replay mechanisms.

- **RabbitMQ.** Choose it for a task queue where messages must be distributed among workers and their processing acknowledged
- **Kafka.** Choose it for a retained event stream read independently by multiple consumer groups

RabbitMQ redelivers a message if the worker connection closes before acknowledgment. Acknowledge it only after `processRequest()` completes successfully. Limit retries and move permanently failing messages to a dead-letter queue.

With Kafka, commit the offset only after successful processing. Limit retries and move permanently failing messages to a separate topic.

The PHP example uses the `AMQPStreamConnection` and `AMQPMessage` classes from php-amqplib. `$requestData` contains validated incoming request data.

{% list tabs %}

- Event Handler

    ```php
    // Publish the message and wait for confirmation before returning success
    use PhpAmqpLib\Connection\AMQPStreamConnection;
    use PhpAmqpLib\Message\AMQPMessage;

    $connection = new AMQPStreamConnection('localhost', 5672, 'user', 'password');
    $channel = $connection->channel();
    $channel->queue_declare('request_queue', false, true, false, false);
    $channel->confirm_select();
    $message = new AMQPMessage(
        json_encode($requestData, JSON_THROW_ON_ERROR),
        ['delivery_mode' => AMQPMessage::DELIVERY_MODE_PERSISTENT]
    );
    $channel->basic_publish($message, '', 'request_queue');
    $channel->wait_for_pending_acks_returns();
    ```

- Worker

    ```php
    // Process queue messages
    use PhpAmqpLib\Connection\AMQPStreamConnection;

    $connection = new AMQPStreamConnection('localhost', 5672, 'user', 'password');
    $channel = $connection->channel();
    $channel->queue_declare('request_queue', false, true, false, false);

    $callback = function($message) {
        processRequest(json_decode($message->body, true));
        $message->ack();
    };
    $channel->basic_consume('request_queue', '', false, false, false, false, $callback);
    while($channel->is_consuming()) {
        $channel->wait();
    }
    ```

{% endlist %}

## How to Choose an Option

Choose a queue based on processing and operational requirements, not only request volume.

| Option | When to Choose It | Failure Handling | Replay | Operations |
|---|---|---|---|---|
| Database | No separate infrastructure is needed; one or more workers process requests | Record statuses and return stalled requests to `pending` | Possible while records remain in the table | Requires lock monitoring and cleanup of processed records |
| Redis | A fast internal queue with a simple processing model is required | Processing list and return of stalled requests to the main queue | Must be implemented separately | Requires a Redis server and memory monitoring |
| RabbitMQ | A task queue, routing, and processing acknowledgments are required | Redelivery of unacknowledged messages and a dead-letter queue | Limited by the queue retention policy | Requires configuring the broker, acknowledgments, and retry rules |
| Kafka | A retained event stream for multiple consumer groups is required | Replay before committing the offset and a separate error topic | Supported within the retention period | Requires managing topics, offsets, and consumer groups |

## Continue Exploring

- [{#T}](./index.md)
- [{#T}](../../api-reference/events/index.md)
- [{#T}](../../api-reference/events/safe-event-handlers.md)
- [{#T}](../cloud-and-on-premise/security-recommendations.md)
