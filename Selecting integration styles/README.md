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

### Module 5 - Choosing appropriate message transformation and routing patterns
 - Data types in REST API can be modeled using JSON/XML schema, RAML and OAS
 - Anypoint Platform supports interoperability between RAML and OAS
 - How to access and populate metadata for mule components
 - Dataweave formats
 - Dataweave can invoke flows, dataweave modules and java methods
 - Java is not generally recommended for transformation. use dataweave instead. However an organization may want to call out to java to reuse a common object model and associated transformations written in java. Dataweave can invoke static methods of java classes. Mule java module can aslo create instance and invoke instance methods.
 - Transformations using scripting: Groovy, Jruby(ruby), Jpython(python) and Nashron(javascript). Not recommended. Excessive use of scripting languages can affect maintainability and understandability.
 - Message enrichment pattern - ways to preserve the previous event payload after an event processor executes - target variable
 - Directly transformation data from one application to another application results in many mappings - O(N2) complexity. Build common (aka canonical) data model CDM - O(N) complexity, decouples multiple transformation steps
 - Data validation pattern - avoid unexpected data errors that could crash downstream applications - validate events as early as possible - dataweave code, modules & connectors(validation module, xml & json schema validators, APIkit router, http request operation performs http response status code validation, java validate type), choice routers(validate without raising error) - all scope and any scope combine validation operations

### Module 7 - Choosing and developing a deployment strategy
 - Control plane - design center, exchange and management center. Runtime plane - runtime engine services
 - Control plane deployment options provided by anypoint platform. Mulesoft hosted - Anypoint platform, aws regions(US and EU) and government cloud(US only)(doesn't include RTF). Customer hosted - Anypoint PCE [Differences table]
 - Runtime plane deployment options provided by anypoint platform. Mulesoft hosted - Cloudhub(cloud based infrastructure, iPaaS, provisioned & managed by Mulesoft), Customer hosted - Runtime fabric(non mulesoft hosted cloud environment(aws, azure and gcp)) and On premises - Standalone runtimes(virtual machines, base metal servers and containers like docker) [Differences table]
 - Mulesoft hosted runtime plane - Cloudhub - Each mule application is deployed to the separate mule runtime that runs in an isolated worker. Cloudhub worker - A new virtual machine is automatically provisioned for the new mule runtime(aws ec2 instance). Horizontal(multiple cloud workers) and Vertical scaling(resize worker to larger or smaller vcore).
 - Load balancing - Distribute inbound http request to multiple cloudhub workers, provides automatic redirection to the new cloudhub worker after mule application is resized or restarted which results in zero downtime, does round robin load distribution across workers, CNAME - load balancer name [For more](https://docs.mulesoft.com/runtime-manager/cloudhub-networking-guide)
 - Distributed object store and persistent vm queues. Persistent object store and persistent vm queues are implemented as a service so data survives restarting/redeploying the mule application
 - Customer hosted runtime plane(either on bare metal servers or in virtual machines) - Allow deployment of multiple mule applications and domains to the same mule runtime - Host multiple mule runtimes
 - Other cloud based infrastructure(aws, azure & gcp) - May not allow deployment of multiple mule applications or mule domains and may not allow hosting multiple mule runtimes on the same virtual machine
 - Runtime manager agent - Securely connect mule runtime with a runtime manager account on the control, to remotely manager and monitor mule runtime and its mule applications
 - Anypoint runtime fabric - A clustered container service that automates and orchestrates the deployment of mule applications to private cloud infrastructure or on-premises data centers - Mule applications deployed to an isolated mule runtime in a docker container, requires the use of control plane hosted by mulesoft - Two deployment options - on VMs/Bare metal(also known as appliance model) and self managed kubernetes(AKS/EKS/GKE)
 - Cluster - A cluster is a set of mule applications that acts as a unit. Severs in a cluster communicate and share information through a Hazelcast-based distribution shared memory grid - Data is replicated across memory in different physical machines - All workers nodes work in the active-active model

### Questions
 - Event Driven Architecture
 - Mule application communicate with a message broker
 - Difference between publish & publish-consume
 - Correlation ID
 - Use cases for using correlation ID

