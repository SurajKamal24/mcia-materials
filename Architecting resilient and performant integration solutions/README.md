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

### Module 11 - Designing transaction management in mule applications
 - Transaction is a group of operations that are guaranteed to all complete or none at all. Traditionally were implemented to meet ACID guarantees. Atomicity, consistency, isolation and durability
 - Database and related systems traditionally uses two-phase commit protocols to manage transactions. Requires transaction manager services
 - Global transactions are transactions that involve multiple separate transactional resources (such as databases)
 - XA interface specifies communication between a transaction manager (TM) and a resource manager (RM)
 - Alternatives to two-phase commit protocols - SAGAS
 - Mule supports single resource (local) tx and global (xa) tx. XA transactions needs an XA capable transaction manager
 - Common connector operations that support transactions. Database - All operations. JMS - publish, consume. VM - publish, consume
 - Begin a transaction using try scope or connectors acting as an event source
 - Committed automatically at the end of flow, try scope and on error continue scope
 - Rollback transaction - After an error occurs in a transaction scope, but only if the error is not handled in an on error continue scope
 - Bitronix is available as an XA transaction manager for mule applications. Each mule runtime can have only one instance of bitronix transaction manager, which is shared by all mule applications
 - SAGA pattern is an alternative to XA when resources are not XA-capable or XA is undesired. For instance API invocations. Event/choreography pattern and command/orchestration pattern
 - JMS acknowledgement - receipt of JMS message must be acknowledged. Unacknowledged messages are redelivered. Auto - automatic aknowledgement at the end of the flow or on error continue scope. Immediate - Automatic acknowledgement upon receipt and not redelivered if processing fails afterwards. Manual - use jms:ack operation. Dups_ok - Similar to auto, optimization as acknowledges messages lazily typically after delay or bulk. Application must handle duplicate message delivery

### Module 12 - Designing for reliability goals
 - Reliability aspires to have zero message/data loss after a mule application stops or crashes. A reliability pattern can be implemented to achieve reliability goals using until successful scope, reconnection strategies, redelivery policy, MULE:REDELIVERY_EXHAUSTED error type, transactions, error handling and first successful router
 - Unitl successful scope repeatedly triggers the code within the scope until succeeds or until a maximum number of retries are exceeded. May fail for temporary reasons and may succeed upon retry. Re-executing permanent failures unneccessarily pollutes logs and delay returing failures
 - System errors are thrown when a connection to an external system fails. To retry connection failures, mule connectors can set a connection strategy
 - A redelivery policy can be configured on the event source to specify the number of the time the same event can be processed by the flow before raising a REDELIVERY_EXHAUSTED error. Can't be configured on the scheduler
 - Reliability pattern for non-transactional system. Splits the processing between acquistion flow and processing flow using the persistent queues
