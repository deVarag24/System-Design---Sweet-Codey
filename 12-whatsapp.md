# WhatsApp System Design

## Functional Requirements
1. One-to-One Messaging
2. Group Messaging
3. Message Delivery Status (Sent, Delivered, Read)
4. Last Seen and Online Status

## Non-Functional Requirements
1. High Availability (99.999999%)
2. Low Latency
3. Scalability
4. Security and Privacy (End-to-End Encryption)

## Capacity Estimation
1. **DAU (Daily Active Users)** - 1B users
2. **MAU (Monthly Active Users)** - 2B unique users
3. **Throughput**
   - **Write -:**
        - Assume each user sends 10 messages per day, resulting in `10B messages/day`.
        - We have 3 type of write request, so `30B write request/day`.
   - **Read -:**
        - Assume each message is read by 2 users on average, resulting in `20B reads/day`.
        - We have 3 type of read request, so `60B read request/day`.
4. **Storage**
   - Assume the average size of a message is 100 bytes and multimedia content is 250KB, And 99% of messages are text messages, and 1% are multimedia messages.
   - Total storage required per day = (99% of 10B messages * 100 bytes) + (1% of 10B messages * 250KB)
    - Total storage required per day = 990GB + 25PB = `25.99 PB/day`
5. **Memory**
   - Assume we store 0.5% of messages in memory for low latency access, resulting in `130GB/day`.
6. **Network Bandwidth**
   - **Ingress -:**
        - 30B write requests/day * 100 bytes = `3 PB/day`
        - 30B write requests/day * 250KB = `7.5 PB/day`
        - Total Ingress = `10.5 PB/day`
   - **Egress -:**
        - 60B read requests/day * 100 bytes = `6 PB/day`
        - 60B read requests/day * 250KB = `15 PB/day`
        - Total Egress = `21 PB/day`
## API Design
### One-to-One Messaging API
1. User A sends a message to User B
2. Server receives the message and stores it in the database
3. Server setup a websocket connection with User B and delivers the message in real-time
4. Server updates the message delivery status (Sent, Delivered, Read) in the database
- **Endpoint -** POST /v1/messages
- **Body**
```
{
    senderId: string,
    receiverId: string,
    messageType: string, // text, image, video, etc.
    content: string, // message content or URL for multimedia content
}
```
- **Response**
```
{
    messageId: string,
    status: string, // Sent, Delivered, Read
}

HTTP Status Code: 101 (Switching Protocols) for WebSocket connection
```

### Message Delivery Status API
use same websocket connection established in One-to-One Messaging API to update the message delivery status in real-time.
- **Endpoint -** GET /v1/messages/status/{messageId}
- **Response**
```
{
    messageId: string,
    status: string, // Sent, Delivered, Read
}
```
### Online Status API
1. We use a websocket connection to send the active status of users in some interval (e.g., every 30 seconds) to the server.

- **Endpoint -** GET /v1/users/online-status/{userId}
- **Response**
```
{
    userId: string,
    onlineStatus: string, // Online, Offline
    lastSeen: timestamp
}
```
### Group Messaging API
We use WebSocket connection to deliver messages in real-time to all group members. The server will maintain a list of group members and their WebSocket connections to deliver messages efficiently.
- **Endpoint -** POST /v1/groups/{groupId}/messages
- **Body**
```
{
    senderId: string,
    messageType: string, // text, image, video, etc.
    content: string, // message content or URL for multimedia content
    groupId: string
}
```

## DB Selection
1. **Message Db** - NoSQL
2. **Group Db** - NoSQL
3. **Connection Db** - Cache
4. **Last Seen Db** - NoSQL

## DB Modeling
1. **Message Db**
```
{
    messageId: string,
    senderId: string,
    receiverId: string, // for one-to-one messaging
    messageType: string, // text, image, video, etc.
    content: string, // message content or URL for multimedia content
    timestamp: timestamp,
    status: string // Sent, Delivered, Read
    assetUrl: string // URL for multimedia content
}
```

2. **Group Db**
```
{
    groupId: string,
    groupName: string,
    members: [string], // list of userIds
    createdAt: timestamp
}
```
3. **Connection Db**
```
{
    userId: string,
    connectionId: string, // WebSocket connection ID
    lastActive: timestamp
}
```
4. **Last Seen Db**
```
{
    userId: string,
    lastSeen: timestamp
}
```

## Deep Dive
1. **End to End Encryption**
    - We use Lock and Key mechanism for end-to-end encryption. Each user has a unique public-private key pair. When User A sends a message to User B, User A encrypts the message using User B's public key. Only User B can decrypt the message using their private key. This ensures that only the intended recipient can read the message, providing end-to-end encryption and ensuring privacy and security of user data.