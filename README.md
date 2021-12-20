# MCIA Material
## Topics
  - Configuring and provisioning Anypoint Platform

## Configuring and provisioning Anypoint Platform
**Modules** - 5,7,9,10
- ### Module 5 - Choosing appropriate message transformation and routing patterns
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

- ### Module 7 - Choosing and developing a deployment strategy
  - Control plane - design center, exchange and management center. Runtime plane - runtime engine services
  - Control plane deployment options provided by anypoint platform. Mulesoft hosted - Anypoint platform, aws regions(US and EU) and government cloud(US only). Customer hosted - Anypoint PCE
  - Runtime plane deployment options provided by anypoint platform. Mulesoft hosted - Cloudhub(cloud based infrastructure, iPaaS, provisioned & managed by Mulesoft), Customer hosted -  non mulesoft hosted cloud environment(aws, azure and gcp) and On premises (virtual machines, base metal servers and containers like docker)
  - Cloudhub - Each mule application is deployed to the separate mule runtime that runs in an isolated worker. Cloudhub worker - A new virtual machine is automatically provisioned for the new mule runtime(aws ec2 instance). Horizontal(multiple cloud workers) and Vertical scaling(resize worker to larger or smaller vcore).
  - Load balancing - Distribute inbound http request to multiple cloudhub workers, provides automatic redirection to the new cloudhub worker after mule application is resized or restarted which results in zero downtime, does round robin load distribution across workers, CNAME - load balancer name [For more](https://docs.mulesoft.com/runtime-manager/cloudhub-networking-guide)
  - Distributed object store and persistent vm queues. Persistent object store and persistent vm queues are implemented as a service so data survives restarting/redeploying the mule application.
  - 
