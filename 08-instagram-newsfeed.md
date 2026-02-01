# Instagram Newsfeed

## Functional Requirement
* Create posts **(Text, Image, Video)**
* Follow and Un-Follow
* News Feed **(Followed Users)** **(New -> Old)**
* Like and Comments
* Notification

## Non-Functional Requirement
* Availability
    - System should be available with **99.999% uptime**
* Eventual Consistency
* Low Latency
* Scalabilty
    - 500M DAU
    - 2B MAU
* Extensibility
    - Easy to extend in the future
* Usability
    - UX (Newsfeed load fast)

## Capacity Estimation
* DAU & MAU
* Throughput
* Storage
* Memory
* Network

### DAU & MAU
* **DAU :-**  500M
* **MAU :-**  2B

### Throughput
* Write to system
    -   create post
        - 10% of DAU Post - **50M Req/Day**
    - follow and un-follow
        - 1% of DAU - **5M Req/Day**
    - Comment & Liking
        -   5% of DAU - **15M Req/Day**
* Read to system
    - View newsfeed
        - Assume a user see 100Post/Day - **5B Req/Day**
### Storage
|                                    | Video | Image | Text |
|------------------------------------|-------|-------|------|
| Avg Size                           | 20MB  | 0.5MB |100KB |
| % of each type of Post             |    20%   |   60%    |   20%   |
| Data Storage per post (50M Req/Day)|   200TB/Day    |   15TB/Day    |   1TB/Day   |

* total Storage **216TB/Day**
* Data Storage for 10 years **750 PB**

### Memory (Cache Memory)
Amount of cache required everyday 1% of daily storage **2TB/Day**

### Network (Bandwidth)
* **Ingress :-** Data flow into our system per sec
    - 216TB/Day = **2.5 GB/sec**
* **Egress :-** Data flow out our system per sec
    - 5B Req/day * (0.2 * 100KB + 0.6 * 0.5MB + 0.2 * 20MB) = **2.5 TB/sec**


## API Design

### Create Post

- **Endpoint -** POST `/v1/post`
- **Body -**
```
{
    userId          String
    mediaType       ("text", "image", "video")
    text?           String
    mediaUrl?       String[]
    description?    String
    hashTag?        String[]
}
```
### Create Comment

- **Endpoint -** POST `/v1/comment`
- **Body -**
```
{
    userId          String
    postId          String
    Comment         String
}
```
### Create Like

- **Endpoint -** POST `/v1/like`
- **Body -**
```
{
    userId          String
    postId          String
}
```
### Follow

- **Endpoint -** POST `/v1/follow`
- **Body -**
```
{
    followBy          String
    followTo          String
}
```
### Un-Follow

- **Endpoint -** PATCH `/v1/un-follow`
- **Body -**
```
{
    followBy          String
    followTo          String
}
```
### News Feed

- **Endpoint -** GET `/v1/feed/:userId`
