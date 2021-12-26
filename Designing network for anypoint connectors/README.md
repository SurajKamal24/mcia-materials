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

### Module 15 - Designing secure mule applications and deployments
 - Anypoint platform security areas and features
