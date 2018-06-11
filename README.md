# Kafka what
- Comes from LinkedIn at 2011, now a separate company
- Used a lot, big folks, local: Unity, Nordea

# Kafka how
- Append only log file
- Persistent - Kafka is a DB!
- Consumer offsets stored in kafka itself
- Partitions
- Topic compaction
- Tumbstones
- Date retention
- Rule: N of Partitions > N of Consumers

# Kafka Streams
Stateful transformation
Table <> Stream

# Kafka KSQL


# Kafka Connect
Mongo >> [Processing] >> Postgres
Which DB are supported

# Kafka REST
Consume topic using CURL

# Event Driven Services
Batch VS RealTime -> Reactive systems
Example - LinkedIn Arch: Kafka as a single Event Bus


Example - Data share: Places service && Recommendation service && Search service
Example - Stored data structure is A, queried data structures are B,C,D..Z
Example - Cache: Customer information, Company information: 0ms response
Example - Hot Cache: Mix cache && REST. Cache invalidation (TODO: Naming...)
Example - Lambda Arch: Batch && Stream

Example - Netflix Arch: ~ 250 Kafka Event Bus
Example - How to split monolith: Convert DB table intro stream
    Two new integrations possible: EventBased
        Batch processes: Only new data from last sync checkpoint
                         Give me all from the start
    Create REST wrapper on top of stream, still value
        + Stop adding more clients to monolith and point to new service
        + Lower load on monolith as essentially it's a DB copy
        + Resilent to monolith errors
        + Only read, but possible to proxy write to monolith
        - Few seconds lag (you cannot read your own writes!)



Example - HTTP on the edge, inside only events
    Batch -> POST /company/register
    vs
    Batch -> Event CompanyRegistrationRequest -> Event CompanyRegistration        -> SendEmailToCompanyAdmin/CreateCompanyDB/UpdateLandingPageCompanies(Silly, important!)
                                              -> Event CompanyRegistrationFailure

# Why not
Error handling
    Stop processing
    Move to bad queue

If latency is important
    One item slows down all following
    You can't send 2 requests in parallel to improve P99

Possible to emulate Requests -> Response, but not the best approach
Ack every message is possible, but not common -> Better accept duplicates
If sensetive to P100 latency and IO is involved -> More work is needed e.g. bad items queue
