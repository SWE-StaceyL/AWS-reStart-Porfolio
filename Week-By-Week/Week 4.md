### AWS re/Start Learning Journey

### Week 4 – AWS Database & Analytics Services

#### 📖 Topics & Services Covered
- Amazon RDS for MySQL & Amazon Aurora
- Amazon DynamoDB — NoSQL at scale
- Amazon DynamoDB Accelerator (DAX) — in-memory cache
- Amazon MemoryDB for Redis
- Amazon DocumentDB (MongoDB compatible)
- Amazon Athena — serverless SQL queries on S3
- Amazon Kinesis Video Streams
- Amazon Data Firehose — real-time data delivery
- Database and Analytics Cheat Sheets

####  Labs & Hands-On Projects
- Explored relational vs NoSQL database architectures
- Reviewed DynamoDB table design and partition key strategies
- Studied Athena for querying data lakes without managing infrastructure
- Compared Kinesis Firehose and Kinesis Data Streams for streaming use cases

####  Challenges & How I Solved Them
- **Challenge:** Deciding when to use DynamoDB vs RDS vs DocumentDB.
- **Solution:** Applied a decision framework: structured data with complex queries → RDS/Aurora; key-value / high-throughput → DynamoDB; document / JSON-heavy workloads → DocumentDB.

####  Key Takeaways & Reflections
> AWS offers a purpose-built database for almost every use case — relational, NoSQL, in-memory, document, and time-series. The analytics layer (Athena, Firehose) enables real-time and ad hoc querying without provisioning servers, which is a powerful and cost-effective pattern for data-driven architectures.

