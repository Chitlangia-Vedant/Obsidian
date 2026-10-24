- **Step 1:** **Define the problem**. Ask clarifying questions to gather more information about the system requirements. Then, estimate the amount of data. (8 minutes)
- **Step 2: Design the system at a high level**. Explain how each part of the system works together. Design the APIs and data model. Then, create a high-level design diagram. (10 minutes)
- **Step 3: Deep-dive into the design**. Examine the system components and their relationships in detail. Assess tradeoffs and evaluate design choices. (10 minutes)
- **Step 4:** **Identify bottlenecks and scaling opportunities**. Zoom out and consider if the system can scale. Incorporate the non-functional requirements through optimization techniques. (10 minutes)
- **Step 5:** **Review and summarize**. Summarize requirements, justify decisions, suggest alternatives, discuss additional tradeoffs, and answer any questions. (4 minutes)

![How to Answer System Design Interview Questions Framework - Lesson](https://images.ctfassets.net/x78yjrjc11pq/2LSflewaGtM5PuNbQoWFwJ/e90a69939ea86e8c67917c57bf8589e2/How_to_Answer_System_Design_Interview_Questions_Framework_-_Lesson.png)
```Example
How would you design Instagram?
```
# Step 1: Define the problem
Asking clarifying questions is a critical first step, as it creates the foundation that your system will be built upon.

Your main objective in this first step is to narrow down what key features and/or aspects of the system your interviewer is most interested in.
Consider assigning priorities, such as P0, P1, etc., to each requirement in order to prioritize your time.

Some sample questions you might ask your interviewer include:

- What specific features are we going to build?
- How many users does the product have?
- Are we creating the system from scratch? Do we need to talk to pieces of the existing system?
- Who are our clients/consumers?

```Example
1. What is the scale of the product we are building (number of users)?
2. How should user feeds be generated? Should they be ordered in any way?
3. What kind of data are we expected to support (text, images, video)?
4. Should we support updating/editing of posts?
```

## Define functional requirements
Identify and discuss the functional requirements of the system, using the insights gained from your clarifying questions. 
Establishing the system’s core functionality will help you determine what is in and out of scope for the specific interview.

```Example
Instagram primarily:

- Allows its wide user base to view and contribute posts on the feed
- Customizes each feed, based on the user’s following

Given its primary features, Instagram’s functional requirements include:

- Users can upload images from a mobile client to create a post
- Users can follow other users
- Users can view a feed of images
```

## Define non-functional requirements
These might be linked to business objectives or the user experience.

Common non-functional requirements include: scalability, latency, security, consistency, reliability, availability, and maintainability.

Some questions you might ask your interviewer are:

- What scale is this system?
- How many users should our app support?
- How many requests should our system handle?
- Are most use cases read-only?
- How important is it that users see accurate, up-to-date data? How important is it that services are available? How important is it for us to horizontally scale?

```Example
Given our functional requirements, it is safe to assume that our non-functional requirements include:

- High availability
- Scalability
- Low latency

Since it’s not a high priority for users to see the most up-to-date data, I will assume it’s okay for the user to see **eventually consistent** data. In this case, I’ll choose availability and fault tolerance over consistency.
```

## Estimate the amount of data
These estimates should help you choose components and give you an idea of what scaling might look like later.

Interviewers generally want to see you make two key calculations:

- Storage: Determine how much data the system will generate, to help inform later decisions around storage solutions.
- Throughput: Determine the number of requests per second (RPS), which you can split into the read and write RPS. Sometimes you may also need to come up with the bandwidth requirements of the system.

To make these estimations, you’ll need to make some assumptions about user volume and typical user behavior.

```Example
To start, I’ll estimate the amount of storage needed to hold a year’s worth of user data.

- 10M Monthly Active Users
- 2 Photos uploaded per month
- 5MB per photo

Result: 10^7 * 2 * 5MB = 10^8MB = 100TB per month = **1.2PB per year**

Next, I’ll compute estimates for the system's QPS number.

- 10M Monthly Active Users
- ~3M Daily Active Users

Write QPS: ~600k photo uploads = 6*10^5/86400 = **~10 uploads per sec**

Read QPS: 10 reads per day * 3M DAU / 86400 = ~**350 reads per sec**
```

# Step 2: Design the system at a high level
Based on the requirements outlined in Step 1, design a high-level system and briefly explain how each piece of the system will work together.

The main objective in this step is to incorporate the requirements you identified in the previous step into your system’s main components.

## Design your APIs
Introduce your system by designing your [APIs](https://www.tryexponent.com/courses/system-design-interview/fundamentals-system-design/system-design-apis) (Application Programming Interfaces).

Each requirement should translate to one or more APIs. You can choose what type of APIs you want to use. Common API types include:

- Representational State Transfer \[REST],
- Simple Object Access Protocol \[SOAP],
- Remote Procedure Call \[RPC], and
- GraphQL

When choosing your APIs, consider the request's parameters and the response type. Clarify which APIs you’ve chosen with your interviewer, and explain why.

```Example
Because our system does not require the server to independently send or push data to the client, we can use standard REST APIs to facilitate communications between the client and the server.

Below we can mock some of our REST APIs mapped from our functional requirements.
```
![](<../../Attachment Folder/Pasted image 20261024061014.png>)

## Design your data model

What data do we need to store in our database and how do we store it?

When designing an effective data model, you need to determine:

1. The type of the data that the system wants to collect/capture (for example, metadata, video files, blob storage, text, video storage)
2. The attributes of the data (for example, does it belong to a specific user? Is the time of submission relevant?)
3. How you’ll store the data, considering the functional requirements
4. How the data will interact with each other, with users/clients, and with the APIs
5. Whether to use a SQL or NoSQL database

```Example
The system wants to collect/capture 3 different data types:

- User data - Metadata relevant to the users of the application
- Photos - Data related to a user uploaded post/photo
- User Followers - Stores the relationships between User-to-User

Specific attributes of the data include:

- Photo entries can trace to a specific user
- User followers have a 1:1 mapping
- Photos table will only store metadata and content is referenceable via URL path

Considering the functional requirements, which prioritize relational querying between photos, users, and followers, I’ll store the data by maintaining a separate table for each data type with foreign key references between them.

Back-end services will perform a JOIN operation on the followers and photo tables in order to generate a feed for each unique user request.

The Instagram product relies heavily on the relation between these data types to support fetching user feeds. A common query would be to fetch all photos for a group of (followed) users. Because of this, it would make sense to use a SQL Database to host our data models.
```

![](<../../Attachment Folder/Pasted image 20261024061417.png>)

## Create a high-level design diagram
The diagram should act as a blueprint of our design and highlight the most critical pieces to fulfill the functional requirements.

Instead of diving into the details of each service, communicate whether the design satisfies all the functional requirements.

```Example
First, we can mock out the core functionality of our system to incorporate all of our functional requirements.

The system below incorporates the following workflows:

1. **Users can upload images from a mobile client to create a post:** Users will issue a request to the write App Server which will write post metadata and the photo to the databases.
2. **Users can follow other users:** similar to above, the write app server will process follow requests and write the relation to the Follows table in the database.
3. **Users can view a feed of images:** In this work-stream the user issues a request to the Read App server which will query the Database for all posts belonging to the users they follow.
```

![](<../../Attachment Folder/Pasted image 20261024061948.png>)
```Example
At this point all the functional requirements should be met. Moving forward, we can identify points of weakness in the current system and how we can address them alongside our non-functional requirements.
```

# Step 3: Deep-dive into the design
Once you have a high-level diagram, it's time to examine system components and relationships in more detail. Walk your interviewer through the lifecycle of a user request to demonstrate that you understand how your system’s various components work together.

The interviewer will likely want to ask questions about the system and how it works to support each functional requirement. Example questions include:

- Can you walk me through the steps on how a user feed is generated in this system?
- What other data storage options can we use to optimize performance?
- What benefit does splitting the write/read servers have on performance?

That being said, don't rely on them to drive the conversation. Check in regularly with your interviewer by asking questions such as:

- Are there specific areas in the design that you have questions or concerns about?
- Would you like me to expand on this, or am I okay to move on?
- How does everything sound so far?

## Assess tradeoffs and evaluate your design choices

If the interviewer doesn’t give you a clear directive, continue the conversation by explaining what tradeoffs you considered and how the system’s requirements impacted your design choices.

Weighing the tradeoffs between different choices to satisfy the system's requirements is considered one of the most critical skill sets in a system design interview.

```Example
Below we can outline some common trade-offs within the context of the problem.

**Relational vs. Non-Relational Database**

To fulfill the feed generation requirement, the system must perform a query on both the “follow” table and our “posts” table. This operation would greatly benefit from the JOIN operation, which is primarily supported by a SQL database.

However, two downsides of using SQL are that we must manually shard our database, and we lose flexibility of our data schema. If these downsides are acceptable for our example, we can still use SQL.

**Distribution vs. Query Performance**

Since SQL is not natively horizontally scalable, we will need to establish a sharding key so that our data will work at scale in a distributed system. Assuming the feed generation will want to prioritize recent posts from followers, a sharding key using both timestamp and user ID would provide the best advantage.

Other requests may be impacted/slowed, but SQL would provide the best performance for our highest priority request.

**Read/Write Service vs. Monolithic App Server**

I de-coupled the write vs. read capabilities into different services for the following reasons:

- They serve very different use cases that will leverage different technologies (e.g. Redis for read server and Kafka for write server)
- This technique minimizes noisy neighbor problems and supports fault tolerance. In the event that the write server has an outage, users can still read and vice versa.
- This technique encourages independent horizontal scaling, where read servers will most likely scale out more than writes. This comes at a tradeoff of additional software engineering complexity, instead of maintaining a monolithic service.
```

# Step 4: Identify bottlenecks and scale

