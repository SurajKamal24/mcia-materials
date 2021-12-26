### Module 2 - Identifying anypoint platform components and capabilities
 - API Specifications - Less technical stakeholders to understand. Encourages visibility and early agreement by less technical stakeholders
 - REST API Specification options - OpenAPI specification(OAS) and REST API Modeling Language(RAML)
 - API Designer - Visual editor - can switch to manual code editor later but cannot go back to the visual editor
 - API Console - REST client and can be accessed in the web browser. Embedded in API designer, API portals in exchanges and Anypoint studio in the API design perspective
 - Behavioral headers - For more mocking use cases like errors, injecting delays, seleting specific examples as response
 - Mule applications can have governance offloaded from code in the mule application to configuration via API manager
 - Event driven API support - [Async API Specification](https://www.asyncapi.com/)
 - Deployment options - Manual, Mule maven plugin, Anypoint studio, Mule agent, Runtime manager UI and Runtime manager REST APIs
 - Deployment targets - Standalone mule runtime, cloudhub, anypoint runtime manager and runtime fabric
 
 ### Module 3 - Designing integration solutions using mule applications
  - Async scope - To call event processors asynchronously and for concurrent processing
  - REST connect feature - For a REST API, a connector is automatically generated at publish time by anypoint exchange
  - Both connectors and modules are built using Mule SDK (Java or XML)
  - REST connector to invoke REST services instead of HTTP connector. Also help Mule applications fail fast to quicky identify issues. Custom connector to invoke SOAP services instead of web service consumer connector
  - Mule API implementation and REST clients use the same REST API specifications to promote parallel development
  - Mule domains can share global elements across mule applications on the same runtime. Not supported in cloudhub nor in runtime fabric
  - Mule 3 - Flows could define a processing strategy to decide if the flow processing was synchronous or asynchronous. Staged Event Driven Architecture
  - HTTP connector - HTTP 1.0 and 1.1 are supported. Grizzly is the standard open source library supporting the HTTPs protocols. HTTP listener - Shared selector pool and HTTP request - Dedicated selector pool
  - Database connector - can call out to almost any JDBC relational databases. Support for DDL, DML, stored procedure, DB script, bulk operations and on table row event source
  - File and FTP/FTPS/SFTP connector
  - Thru MFT is a mulesoft certified third party connector - Manage file transfer processing model - OptiPaaS
  - Types of anypoint connectors - Community, Mulesoft certified, Select and Premium
  - Scheduler - Fixed frequency and cron expression. Database on table row and File, FTP, FTPS, SFTP on new or updated file - Handle watermarks and include a scheduling strategy to trigger a flow
  - Scheduler behaviour Customer hosted or Anypoint runtime fabric - Apps deployed to a cluster run only on the primary node. Standalone apps runs in all mule runtime. Cloudhub   - Apps deployed to multiple workers guarantees that the scheduler execution on a single worker and subsequent triggers might execute on a different worker. Uses UTC timezone and minimum recommended scheduling frequency is 10 secs
  - Raise error can throw standard(MULE:CONNECTIVITY) or custom error type(ORDER:INVALID_DATA), gobal error handler, mule default error handler, application level, flow level and processor level, on error propagate scope, on error continue scope, try scope and system errors are not caught
  - Routers - Choice router, first successful, round robin and scatter gather

### Module 4 - Choosing appropriate mule 4 event processing models
 - Processing Models Mule 4 runtime - Non-blocking and concurrent. Mule 4 components and flows - Synchronous & Asynchronous. Parallel, streaming, iterative, real time and scheduled
 - Reactive programming in mule runtime is an internal product detail that automatically improves performance. Typically does not involve any tuning by developers or administrators
 - Mule 4 applications are automatically configured to handle back pressure via internal mule runtime details. Back pressure can further influenced manually by setting maxconcurrency attribute of the flow
 - Mule 4 execution types - IO_INTENSIVE(blocks most of the time - DB connector and transaction scope), CPU_LITE(100% non-blocking and typically takes less than 10ms. Message passing, filtering and routing) and CPU_INTENSIVE(Typically takes more than 10ms and less than 20% of the processing time. Scripting or dataweave transformations. Never automatically predicted)
 - Mule 4.1 and 4.2 uses a separate thread pool for each execution type. this is called dedicated model. Starting with mule 4.3 only uber thread pool is used for all three execution types. This reduces the likelihood of thread exhaustion errors. Scheduler-pools.conf file - to change the thread pool model
 - Synchronous processing - one dedicated thread, blocks & waiting, entire request & response is handled synchronously in one thread
 - Some connectors and mule components might manager their own threads and thread pools. Independent of the thread pools uber and dedicated
 - HTTP - synchronous, stateless, hypermedia type data, client-server architecture
 - REST - Addressable resource, stateless, connectionless, uniform interface, idempotence
 - SOAP - XML and W3C standards, uses WSDL and supports HTTPS. WS-Security(signature & encryption), WS-AtomicTransaction(ACID) and WS-ReliableMessaging(asynchronous processing)
 - SOAP VS REST SOAP - named operations. REST - standard http methods
 - Event Driven Architecture - Asynchronous exchange of events. Consists of event producers, event consumers and message brokers
 - JMS(Java Messaging Service) - Supports one-to-one(Queue) and one-to-many(Topic or pub-sub model) exchanges of messages
 - JMS connector - Requires adding a JMS client library for the particular JMS provider as a maven dependency. Publish, consume, on new message and publish-consume. Termed as full messaging solution
 - Anypoint MQ - Standard queue, FIFO queue and message exchange. Reliability is provided via queues through lock and ack mode. DLQ - receives only undelivered messages. Should be assigned to the main queue. Both queues must be of the same type and in the same geographical region
 - VM connector - Unlike jms, vm do not use intermediate message broker. Supports intra-app and inter-app(if the configuration is in mule domain). Transient or persistent queues. To distribute and load balance messages acroos a cluster of mule runtimes and an application deployed to multiple cloudhub workers. For high performance async communication within a mule application, But not strictly round-robin, load balancing algorithm cannot be changed or tuned
 - Async flow - do not use the outer flow's error handler
 - Streaming options - File-stored repeatable(512kb in-memory buffer), in-memory repeatable and non repeatable streams
 - For each scope, parallel for each scope and batch job.
 - Batch job - Persistent queues- Records always procceeds batch steps in order. Later records can jump ahead to the next batch step while earlier records are still being processed by another thread in the previous batch step. Accept expression, accept policy and variables. Batch aggregator. Payload after the completion of the batch job is BatchJobResult object

### Module 8 - Designing with appropriate state preservation and management options
 - Ways mule application can maintian state - Mule event, object store, vm queues, batch job scope queues, file based persistence and external data store
 - Object store is typically accessed via a object store connector. But an externalized object store implementation might provide other communication mechanisms, such as a secure REST connection. Configured as persistent or non-persistent
 - Mule applications deployed to customer-hosted or runtime fabric could use the OSv2 REST API to access a cloudhub-deployed application's persistent object stores
 - Private object store can be configured by a particular component in a mule application to hide its object store data from any other component in the mule application
 - JMS, AMQP, Anypoint MQ or DB - More durable and more reliable state management option. Communication between multiple mule applications or with non-mule applications
 -  File based persistence cloudhub - /tmp, runtime fabric - /tmp and customer hosted - /tmp and /otherDirs
 -  External store - database, sftp server, redis, s3 bucket and external in-memory grid
 -  Comparisons
 -  OSV2 - unlimited keys, 10mb max value size. Preserved upon application redeployment and restarts. Deleted upon application deletion. 30 days and 10 TPS per app for free tier
 -  Currently OSv2 uses dyanamo DB and vm queue uses amazon sqs
 -  Customer hosted state management - Performance profile and reliable profile
 -  Batch queue bypass the cluster-replication and do not use hazel cast
 -  Cache scope - Does not cache consumable payload such as non repeatable stream
 -  Watermark
 -  Idempotent message validator

### Module 10 - Designing an efficient and automated software development lifecycle
 - Runtime manager properties tab can override values set in properties files. The properties tab is available for both mulesoft-hosted and customer-hosted runtime plane targets
 - System level properties can be set in the wrapper.config file or from the command line when starting the mule runtime
 - External configuration files make mule application easier to migrate and update. Can externalize property file outside the mule runtime for customer hosted mule runtime. External configuration systems can also be used but require more work to set up and manage - Hashicorp vault, Azure vault and spring cloud config
 - Properties hierarchy - Deployment > System > Environment > Application > Global
 - Mule maven plugin - Automate building, packaging and deployment of mule applications from source projects. Munit maven plugin - Automate test execution and ties in with the mule maven plugin. compile > test > package > install > deploy
 - Automation on Anypoint Platform - Anypoint CLI and Anypoint Platform APIs

### Module 12 - Designing for reliability goals
 - Reliability aspires to have zero message/data loss after a mule application stops or crashes. A reliability pattern can be implemented to achieve reliability goals using until successful scope, reconnection strategies, redelivery policy, MULE:REDELIVERY_EXHAUSTED error type, transactions, error handling and first successful router
 - Unitl successful scope repeatedly triggers the code within the scope until succeeds or until a maximum number of retries are exceeded. May fail for temporary reasons and may succeed upon retry. Re-executing permanent failures unneccessarily pollutes logs and delay returing failures
 - System errors are thrown when a connection to an external system fails. To retry connection failures, mule connectors can set a connection strategy
 - A redelivery policy can be configured on the event source to specify the number of the time the same event can be processed by the flow before raising a REDELIVERY_EXHAUSTED error. Can't be configured on the scheduler
 - Reliability pattern for non-transactional system. Splits the processing between acquistion flow and processing flow using the persistent queues

### Questions
 - How could a mule application kickoff processing when a file is created - On new or updated file
 - What is watermark and where its stored

