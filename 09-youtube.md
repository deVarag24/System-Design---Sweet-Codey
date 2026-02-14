# Design YouTube

## Functional Requirement
1. Viewer
    - Streaming
    - Divce Compatibility
2. Upload & Notification

## Non-Functional Requirement
1. Viewer
    - Low Latency
    - Scale
    - UX
    - Availability **(99.999999%)**
2. Upload
    - Scalability
    - Security
    - Storage Reliability

## Capacity Estimation

1. **DAU -** 100M
2. **MAU -** 2.5B
3. **Throughput**
    - **Write (Assume, 1 out of 250 user upload in a day)** 0.4M upload Request/Day
    - **Read (Assume, 1 user watch 10 videos in a day)** 1B read Req/Day
4. **Storage -** Assume avg size of video `600MB`
    - 240TB/Day
    - 876 PB for 10 years
5. **Memory -** Assume `1%` of total daily storage should be cache 
    - 2.4TB/Day
6. **Network Bandwith**
    1. **Ingress -** 
        - 240TB/Day
        - `2.7 GB/sec`
    1. **Egress -** 
        - 1B Req/Day * 600mb = `600PB/Day`
        - `7 TB/sec`

## API Design