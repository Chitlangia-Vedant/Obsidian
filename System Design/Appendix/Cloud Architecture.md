The main difference between **on-premise** and **cloud-based** solutions is _where_ the hardware that runs your software resides. On-premise means that your software runs locally, on machines you own, or that you rent space from within a data center. Cloud-based applications are hosted and maintained entirely by someone else. There are many cloud service providers including the big three:

- Amazon Web Services (AWS)
- Google Cloud (GCloud)
- Microsoft Azure

# Why pursue a cloud-based strategy?

The pros of migrating to the cloud include:

- **Affordability.** Most providers don't charge upfront; you'll likely "pay-as-you-go" and maintenance costs are included.
- **Professional maintenance.** Compatibility and upgrades are taken care of for you, and new software deployments can happen much more quickly.
- **Security.** Huge cloud providers benefit from economies of scale, especially in security. Data centers are able to provide security far beyond what is financially possible for most smaller companies, so you know your data will be safe.
- **Scalability.** The entire cloud model is based on the principle of "only pay for what you need." Need to scale up quickly? No problem.

This all sounds great, but there are cons to consider as well, including:

- **Higher long-term costs.** You'll save a lot upfront, but eventually, you would have paid off your initial hardware costs. Hosting your software on the cloud often increases your total cost of ownership on a long-term basis.
- **Loss of control.** Cloud providers are very flexible, but it's possible that a very complex deployment won't be compatible with a cloud configuration.
- **Vendor lock-in.** It can be very difficult to switch providers or take your software off of the cloud, should you decide that you made the wrong choice.
- **Industry-specific regulations.** Some cloud-based services might not be compliant with industry-specific regulations like HIPAA (healthcare), FERPA (education), PCI (payments).
- **Physical location.** You might need to be physically located in a particular location for legal or security reasons. For example, the US government wouldn't store data in Russia.
- **Airgapping**. If you have ultra-high security needs, you would want to ensure that your system is airgapped - that is, it isn't connected to any external networks.

# What do cloud providers offer?
- **Compute products** in the form of dedicated servers, virtual machines, GPUs, batch processing, or all of the above. There are endless configurations available to suit your needs. You can even go with **serverless computing,** which is event-driven and paid per execution for ultimate flexibility.
- **Containers** for your code, isolating it for easy portability and added security.
- **Database flexibility.** All major providers offer many different database solutions, from traditional RDBMS to graph and time-series databases, in-memory database and caching services, and more.
- **Networking.** Including tools to maintain microservices, API gateways, DNS, and more.
- **Niche tools.** In addition to all manner of monitoring and analytics, you'll have access to developer tools, machine learning, and all varieties of application-specific computational resources you can imagine.

| Product                                   | AWS                                                                                         | GCloud                                                                | Azure                                                                                     |
| :---------------------------------------- | :------------------------------------------------------------------------------------------ | :-------------------------------------------------------------------- | :---------------------------------------------------------------------------------------- |
| Compute & Compute Autoscaling             | AWS EC2, AWS Autoscaling                                                                    | Cloud GPUs, Compute Engine, Compute Engine Autoscaler                 | Azure Virtual Machines (VM), Azure Autoscale, Azure VM Scale Sets                         |
| Load Balancing                            | Elastic Load Balancing                                                                      | Cloud Load Balancing                                                  | Azure Load Balancing                                                                      |
| Serverless Computing                      | AWS Lambda                                                                                  | Cloud Functions                                                       | Azure Functions, Serverless Compute                                                       |
| Containers                                | Elastic Container Service (ECS), Amazon Elastic Kubernetes Service (EKS), AWS App2Container | Google Kubernetes Engine, Migrate for Anthos                          | Azure Kubernetes Service (AKS), Azure Migrate                                             |
| Object Storage and Database               | S3, Amazon DynamoDB (NoSQL), Amazon Aurora, Amazon RDS (SQL)                                | Cloud Storage, Datastore, Cloud Bigtable (NoSQL), Cloud Spanner (SQL) | Azure Storage, Azure Cosmos DB (NoSQL), Azure SQL Database, Azure Database for PostgreSQL |
| Data Analytics                            | AWS Glue, Amazon AppFlow, Amazon Data Pipeline                                              | Data Catalog, Cloud Data Fusion                                       | Azure Purview, Azure Data Factory                                                         |
| Logging, Monitoring & Performance Tracing | Amazon CloudWatch, CloudWatch Logs, AWS X-Ray                                               | Cloud Logging, Cloud Monitoring, Coud Trace                           | Azure Monitor, Monitor Logs, Monitor Application Insights, Distributed Tracing            |
| Platform-as-a-Service (PaaS)              | AWS Elastic Beanstalk                                                                       | App Engine                                                            | Azure App Service                                                                         |
# Orchestration services
Orchestration automates the processes needed to monitor and manage a complex, auto-scaling cloud deployment. Two popular orchestration tools to know are **Terraform** and **Kubernetes**. Both of these are _open-source_ which is a huge plus when migrating between cloud platforms. They can't be used interchangeably as they excel at orchestrating different "layers" of a complex system — but there is overlap, and they can be used together.

Terraform manages the physical infrastructure of an auto-scaling system. This includes DNS records, virtual machine (VM) instances, etc. Generally lower-layer resources.
It's an instance of a clever tactic called **Infrastructure as Code**. The idea here is that by treating your infrastructure as code, you're able to easily document changes. Roll back a version if needed, or scale globally - anything you can do with code, you can pretty much do with your physical infrastructure using Terraform.

Kubernetes, a **container orchestration platform**, manages upper-layer resources. Building containerized applications has several advantages - each container is isolated from one another, making development, scaling, and maintenance all relatively simple. But as you can imagine, managing a large network of containers can be challenging. Kubernetes treats groups of containers as clusters, and manages the different workflows required by each cluster as needed.

![](<../Attachments/Pasted image 20261104135346.png>)

# How companies switch from on-premise to cloud-based hosting

It's worth noting that there are a few different strategies businesses can take when deciding to migrate to the cloud. Here are a few popular choices.

**1) Re-host:** Also known as "lift and shift." Simply shift data from on-premise infrastructure to a cloud-based infrastructure. Good for large-scale migrations.

- Pros: Simple, fast, automation tools available
- Cons: Legacy apps might be incompatible, may miss out on some benefits of the cloud

**2) Re-platform:** "Lift, tinker and shift." Rehosting with some minor tweaks to capture full cloud benefits.

- Pros: Capture all the benefits of cloud migration with minimal effort
- Cons: Careful management is needed as re-platforming can easily turn into a full, time-and-resource intensive refactoring.

**3) Refactor**: Fully re-architecting applications to suit the cloud. Often used only as a last resort, or as a strategic choice (for example, when choosing to switch from a monolithic to microservice architecture.)

- Pros: Refactoring at the right time can be a huge boost to a company losing its competitive edge. Successful refactoring means increased speed, scalability, and performance.
- Cons: Risky. Easily the most difficult, expensive, and time-consuming strategy. Can be hugely disruptive to operations if mismanaged.

**4) Repurchase:** A much lighter migration; from an on-premise application to a cloud-based solution. Good for transferring standard business workflows like payroll and accounting or CMS.

- Pros: Simple, fast, and easy
- Cons: Few major risks apart from "switching costs" from having to retrain staff and exit an existing contract

# Further reading
- When Spotify began to consider switching to the cloud in 2015, they had no idea how complex the project would be. Read [this series](https://engineering.atspotify.com/2019/12/09/views-from-the-cloud-a-history-of-spotifys-journey-to-the-cloud-part-1-2/) from their engineering blog to learn about the technical details as well as the business case for switching.
- Check out this [exhaustive comparison table](https://cloud.google.com/free/docs/aws-azure-gcp-service-comparison) of Cloud products by function for GCloud, AWS, and Azure (take with a grain of salt; it's published by Google!)