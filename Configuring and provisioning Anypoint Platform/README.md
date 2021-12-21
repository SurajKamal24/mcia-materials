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

### Module 9 - Designing effective logging and monitoring
> API Analytics and API Monitoring
 - Access management contains the audit log. Organization owner can access all audit logs. Can be used to track access violations. Retained in anypoint platform for 6 years
 - Log levels - DEBUG(async), TRACE(async), INFO(async), WARN(sync) and ERROR(sync)
 - Cloudhub truncates log messages - Upto 100 MB per mule application and per worker - At most 30 days
 - Difference between cloudhub and customer hosted runtime planes - System.out messages will be available in application logs for cloudhub but available in system logs for customer hosted runtim plane
 - Runtime manager can override the log level for a deployed mule application without redployment :triangular_flag_on_post:	
 - Tracing log messages using correlation ID (X-Correlation-ID)
 - By default cloudhub replaces a mule application's log4j2.xml file with the a cloudhub provided log4j2.xml file. Send logs(only application logs and not system logs) to external logging systems using a custom log appender by raising a request to mulesoft support to get the option of disabling cloudhub logs
 - As another option, create a custom aggregator application which fetch logs using cloudhub APIs and send to an external system - Generally not recommended
 - Anypoint monitoring. Platinum - Application performance monitoring and custom metrics & events. Titanium - Application performance monitoring, log management, custom metrics & events, dedicated monitoring infrastructure and enhanced support.
 - Cloudhub - enable/disable via anypoint monitoring and runtime manager. On-premises - Anypoint monitoring agent. Runtime fabric - By default all applications are enabled for anypoint monitoring.
 - Default built in dashboard - Inbound & outbound events, Performance, Infrastructure, JVM and Failures
 - Alerts - Basic(Different metrics are available sources(Mule applications vs servers)) and Advanced(Can be created on widgets from custom dashbaords). Can send notification to email addresses. These alerts are distinct from API manager and runtime manager alerts
 - Custom metrics connector can generate metrics from a flow
 - Monitoring applications from public or private locations
 - Anypoint analytics - Request by date, location, application and platform. Custom dashboards - create charts which displays request, request size, response size and response time
 - Alerts can be configured in runtime manager. Custom notifications can be generated by a mule application using the cloudhub connector, which can generate custom alerts in runtime manager
 - Runtime manager doesn't support alerting on RTF deployed applications.

### Module 10 - Designing an efficient and automated software development lifecycle
 - Runtime manager properties tab can override values set in properties files. The properties tab is available for both mulesoft-hosted and customer-hosted runtime plane targets
 - System level properties can be set in the wrapper.config file or from the command line when starting the mule runtime
 - External configuration files make mule application easier to migrate and update. Can externalize property file outside the mule runtime for customer hosted mule runtime. External configuration systems can also be used but require more work to set up and manage - Hashicorp vault, Azure vault and spring cloud config
 - Properties hierarchy - Deployment > System > Environment > Application > Global
 - Mule maven plugin - Automate building, packaging and deployment of mule applications from source projects. Munit maven plugin - Automate test execution and ties in with the mule maven plugin. compile > test > package > install > deploy
 - Automation on Anypoint Platform - Anypoint CLI and Anypoint Platform APIs

### Questions
- Reason to configure business groups - Business groups provides a way to seperate and control access to the anypoint platform resources because the anypoint platform users should have access only to the business group in which they have a role
- Identity management - Set up users for signle sign on (SSO), OpenID connect & SAML 2.0. Default is Anypoint platform. Client management - Authorize client applications which try to access your API
- Public worker cloud - SLB. Cloudhub VPC - DLB
- Highly available - Multiple workers and nodes
- Things can be done on customer hosted mule runtime but not on runtime fabric or cloudhub - Domain project, multiple mule applications per runtime, runtime tuning
- Configuring a VPC
- Things cannot be done in cloudhub without a VPC - Connection to onpremise resources - IPsec VPN, AWS VPC peering and AWS direct connect - Custom domains, custom certificates, mutual tls and load balancer rate limits
- Concurrency issues resolved by cluster
- Advantage of automatic load balancing within a cluster
- what is shared between cluster nodes - Datagrid

