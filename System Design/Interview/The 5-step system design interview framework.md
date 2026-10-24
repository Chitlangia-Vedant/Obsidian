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