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
  - Scheduler behaviour Customer hosted or Anypoint runtime fabric - Apps deployed to a cluster run only on the primary node. Standalone apps runs in all mule runtime. Cloudhub - Apps deployed to multiple workers guarantees that the scheduler execution on a single worker and subsequent triggers might execute on a different worker. Uses UTC timezone and minimum recommended scheduling frequency is 10 secs
  - Raise error can throw standard(MULE:CONNECTIVITY) or custom error type(ORDER:INVALID_DATA), gobal error handler, mule default error handler, application level, flow level and processor level, on error propagate scope, on error continue scope, try scope and system errors are not caught
  - Routers - Choice router, first successful, round robin and scatter gather
  -  
