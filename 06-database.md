# Database And Storage

1. **SQL/Relational DB**
    - DB which stores data in tabular form
    - data store in row and columns
    - data is well structured format
    - *eg :-* MySQL, PostgreSQL

2. **NoSQL/Non-Relational DB**
    - DB which stores data in documented format
    - data is unstructured format
    - they have many type as per the data
        - Key-Value store
        - document DB
        - Graph DB
        - Wide column DB
        - Time Series DB
        - etc......
    - *eg :-* MongoDB, Cassandra, DynamoDB
    - they are highly scalable and flexible

## How to choose between them

1. **Speed** NoSQL
2. **Scale** NoSQL
3. **Data Structure**
    - **fixed** SQL
    - **flexible** NoSQL
4. **Complexity of queries** SQL

### SQL DB
- Structured data
- complex queries
- consistent data

### NoSQL DB
- Unstructured data
- Large scale app
- flexibility and speed

## Object Storage

- Which store large amount of data
    - videos, files, audios, photos
- Data is store in object
- services that offer object storage
    - Amazon S3, Google Cloud Storage, Azure Blob Storage

## Cache
- Super fast Memory
- store in ram
- limited capacity
- store frequently used data
- **Cache miss**
    - if data not found in cache and it go to db

## Content Delivery Network (CDN)
If a server is one side of world and user try to access that server from other side of world, if content has large files like video, photo, then it travale through long distance which make response slow

- **CDN**
    - Store server's static content

*CDN store copies of such data in verious location of world and provied data from least distance place*
