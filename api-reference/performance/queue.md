# Incoming Event Queue

The concept of an **"incoming queue"** for processing HTTP requests implies that you do not handle requests to your event handlers immediately as they arrive, but instead place them in a queue from which they are sequentially retrieved and processed by your server or a group of servers.

### Main Principle:

1. **Requests arrive at the server**: When Bitrix24 sends HTTP requests to your event handler, they first reach the server or load balancer. In response to the incoming request, your server immediately replies that the request has been accepted and is being processed (HTTP 200/202). Thus, from the perspective of the event queue, your handler operates as quickly as possible, which means it will be called with maximum priority in the future.
2. **Requests are added to the queue**: Instead of immediate processing, you must save the data from the request in a special queue (for example, in a database, cache system, or distributed queue).
3. **Retrieving requests from the queue**: Regardless of incoming requests, one or more handlers (workers) retrieve requests from the queue and process them one by one or in parallel, depending on available resources.

### Advantages:

1. **Load management**: Queues allow for sequential processing of requests and prevent server overload during traffic spikes.
2. **Increased stability**: Even if the system experiences high load, requests are not lost but queued and processed later.
3. **Scalability**: Queues make it easy to scale the system by adding more workers or servers to handle requests.
4. **Improved response for clients**: Clients can immediately receive a message that their request has been accepted, rather than waiting for processing to complete.

### Simple Implementation Option:

#### 1. **Saving to a Database (DB-based Queue)**

The simplest way is to save incoming requests to a database.

- **Requests arrive at the server**, and the request information (body, metadata) is saved to the database.
- A **background process** or cron job regularly checks the database for new requests, processes them, and updates their status.

Example:

1. Bitrix24 sends a request to your event handler, and the request is saved in the `requests` table.
2. A background task checks the table every few seconds and processes requests.

**Advantage**: Simplicity of implementation.

**Disadvantages**: Possible performance degradation under high loads due to contention for database access.

Example in PHP:

{% list tabs %}

- Event Handler

    ```php
    // Save the request to the database, do nothing else!
    $db->query("INSERT INTO requests (data, status) VALUES ('$data', 'pending')");
    ```

- Worker

    ```php
    // Select a batch of records from the queue for processing
    $pendingRequests = $db->query("SELECT * FROM requests WHERE status='pending'");
    foreach ($pendingRequests as $request) {
        // Process the request
        processRequest($request);
        $db->query("UPDATE requests SET status='processed' WHERE id=".$request['id']);
    }
    ```

{% endlist %}

#### 2. **Using Redis (or Memcached)**

To improve performance, you can use **Redis** or **Memcached** as a simple queue.

- Incoming requests are queued in Redis using the `LPUSH` command (add an element to the list).
- Handlers (workers) retrieve requests using `RPOP` (take an element from the end of the list) and process them.

**Advantages**: Fast operation, especially with large volumes of data and high loads.

Example in PHP:

{% list tabs %}

- Event Handler

    ```php
    // Save the request to the database, do nothing else!
    $redis->lPush('request_queue', json_encode($requestData));
    ```

- Worker

    ```php
    // Select a batch of records from the queue for processing
    while ($request = $redis->rPop('request_queue')) {
        processRequest(json_decode($request));
    }
    ```

{% endlist %}

#### 3. **RabbitMQ or Kafka (Advanced Solution)**

For more complex and larger systems, you can use queue systems like **RabbitMQ** or **Kafka**.

- **RabbitMQ**: This is a popular queue system that supports complex message routing scenarios, load balancing, and scaling. RabbitMQ supports message acknowledgment, ensuring their delivery.
- **Kafka**: Used in large systems for stream processing. Kafka allows distributing data among many consumers, enhancing scalability and reliability.

**Advantages**: High scalability, reliability, and the ability for complex routing and load balancing.

Example of using RabbitMQ in PHP:

{% list tabs %}

- Event Handler

    ```php
    // Sends a message to the RabbitMQ queue, do nothing else!
    $connection = new AMQPStreamConnection('localhost', 5672, 'user', 'password');
    $channel = $connection->channel();
    $channel->queue_declare('request_queue', false, true, false, false);
    $channel->basic_publish(new AMQPMessage(json_encode($requestData)), '', 'request_queue');
    ```

- Worker

    ```php
    // Process messages from the queue
    $callback = function($msg) {
        processRequest(json_decode($msg->body));
    };
    $channel->basic_consume('request_queue', '', false, true, false, false, $callback);
    while($channel->is_consuming()) {
        $channel->wait();
    }
    ```

{% endlist %}

### Recommendations for Implementing an Incoming Queue

1. **For small systems**: You can start with a database to save requests, which is simpler and easier to implement.
2. **For systems with moderate load**: Use Redis or Memcached to improve performance and real-time request processing.
3. **For large and high-load systems**: Consider using specialized solutions like RabbitMQ or Kafka to ensure high availability, scalability, and processing of large volumes of data.