An _Application Programming Interface (API)_ is a way for different systems to talk to each other.

An _Application Programming Interface (API)_ is a way for different systems to talk to each other.

# How APIs work
APIs deliver client requests and return responses via JavaScript Object Notation (JSON) or Extensible Markup Language (XML), usually over the Internet (web APIs). Each _request-and-response cycle_ is an _API call_.

A request typically consists of a server endpoint Uniform Resource Locator (URL) and a request method, usually through HTTP. The request method indicates the desired API action. An HTTP response contains a _status code_, a _header_, and a _response body_.

Common status codes include:

- `200` (OK)
- `401` (unauthorized user)
- `404` (URL not found)
- `500` (internal server error)

The response body varies depending on the HTTP request, which could be the server resource a client needs to access or any application-specific messages.

To streamline communication, APIs often converge on a few popular specifications to standardize information exchange. By far, the most common is _REpresentational State Transfer (REST)_.

# REST APIs
_REpresentational State Transfer (REST)_ APIs focus on _server resources_ and limit the set of methods to those based on HTTP methods that access resources.

The most common request methods are:

- `GET` (to retrieve)
- `POST` (to create)
- `PUT` (to update)
- `DELETE` (to delete/remove)

REST APIs access _resources_ through URLs, just like the URLs in your browser. The URLs are accompanied by a method that specifies how you want to interact with the resource.

![What is a REST API?](https://images.ctfassets.net/x78yjrjc11pq/3lHJy6ckt0EbTVh0oj3Oq4/9834f1624a32ec8b18b287d3ddcdb246/What_is_a_REST_API)

REST APIs focus on _resources_ rather than database entries.
# Other popular APIs
Other than REST APIs, there are other popular APIs, such as Remote Procedure Call ([RPC](https://en.wikipedia.org/wiki/Remote_procedure_call)), [GraphQL](https://graphql.org/), and Simple Access Protocol ([SOAP](https://en.wikipedia.org/wiki/SOAP)):

## RPC

A requester selects a remote procedure to execute, serializes the parameters, and then sends the lightweight binary data to the server. The server decodes the message and executes the operation, sending the result back to the requester.

Its simplicity and light payloads have made it a de facto standard for many inter-service data exchanges on the backend.

## GraphQL

GraphQL was developed to allow clients to execute precise queries and retrieve only the data they are interested in, typically from a graph database.

To achieve this process, servers need to predefine a schema that describes all possible queries and their return types. This reduces the server payload and offers the client a great amount of flexibility during query time.

However, performance can suffer when the client has too many nested fields in one request. Additionally, there is a steep learning curve that requires extensive knowledge.

Therefore, GraphQL users need to find a balance between its benefits and implementation costs.

## SOAP

A precursor to REST. SOAP works like an envelope that contains a bulky, text-based message. It is much slower than binary messaging protocols, such as RPC.

However, the standardized text format and security features make enforcing legal contracts and authentication easy throughout the API's processing.

SOAP is a legacy protocol and is still heavily used in high-security data transmission scenarios, such as billing operations, booking, and payment systems.

# API design patterns
_API design patterns_ provide a set of design principles for building internal and public-facing APIs. 
## Pagination

One user experience design pattern uses _pagination_, where users can navigate between different search results pages using links, such as "next,” "previous,” and a list of page numbers.

**How do we implement a search query API that supports pagination?**

For simplicity, let's assume that Google stores the result of the queried word "Wikipedia" at `https://www.google.com/some_storage_path/wikipedia/` .

Our API request to fetch the search result for "Wikipedia" would look like this:

`GET “[https://www.google.com/some_storage_path/wikipedia](https://www.google.com/storage/wikipedia/)?limit=20”`

- The `limit=20` parameter means that we only want to list the first 20 results on the first page.
- If everything goes well, our API request receives a response with a `200` `OK` status code and a response body containing the first 20 pages that match our search word "Wikipedia".
- The response also contains a link to the next page that shows the following 20 available matching pages, and an empty link to the previous page.
- Then, we can send a second API request asking for the following 20 pages by specifying the offset of the search results.

Use the following `GET` request to retrieve the first page of results:

- The response body could contain the results for the first page, a valid link to the next page, and an empty previous page link. If we have exhausted all the result pages, the following page link will be empty.
- It is good design practice to add pagination from the start. Otherwise, a client that is unaware of the current `GET` API that supports pagination could mistakenly believe they obtained the full result when in fact, they only obtained the very first page.

In addition to pagination as a solution to navigate between a list of pages, other design patterns include _load more_ and _infinite scrolling_. Learn more about [pagination vs. load more vs. infinite scrolling design patterns](https://crocoblock.com/blog/pagination-vs-infinite-scroll/).

## Long-running operations

When using the `DELETE` API, an appropriate return value depends on whether or not the `DELETE` method immediately removes the resource. If it does, the server should return an empty response body with an appropriate status code, such as `200` (`OK`) or `204` (`No Content`).

For example:

- If the object the client tries to delete is large and takes time to delete, the server can embed a long-running operation in the response body so that the client can later track the progress and receive the deletion result.
- When the `POST` API creates a large resource, the server could return a `202` status code for `Accepted` and a response body indicating that the resource is not yet ready for use.

## API Idempotency

A client may experience failures, such as timeouts, server crashes, or network interruptions, while trying to pay for a service. If the client retries the payment and is double-charged, it could be due to any of the following scenarios:

There are 3 potential scenarios when a payment request is made:

1. The initial handshake between the client and server could fail, preventing the payment request from being sent. In this case, the client can retry the request and hope for success without risking double charging.
2. The server receives the payment request but has not yet begun processing it when a failure occurs. The server sends an error code back to the client, and there is no change to the client's account.
3. The server receives the payment request and successfully processes it or is processing it when a failure occurs. However, the failure happens before the server can return the `200` status code to the client. In this scenario, the client may not realize that their account has already been charged and may initiate another payment, leading to double charging.

Overall, the first two scenarios are acceptable, but the last scenario is problematic and could result in unintentional double charging.

### **Possible solution**

To address this problem, we need to develop an idempotent payment API. An idempotent API ensures that only the first call generates the expected result and subsequent calls are no-ops.

1. Require clients to generate a unique key in the request header when initiating a request to the payment server.
2. The key is sent to the server along with other payment-related information.
3. The server stores the key and process status.
4. If the client receives a failure, it can safely retry the request with the same key in the header, and the payment server can recognize this as a duplicate request and take appropriate action.

For example, if the previous failure interrupted an ongoing payment processing, the payment server might restore the state and continue processing.

_Non-mutating APIs_, such as `GET`, are generally idempotent.

_Mutating APIs_, such as `PUT`, update a resource's state. This means that executing the same `PUT` API multiple times updates the resource only once, while subsequent calls overwrite the resource with the same value. Therefore, `PUT` APIs are usually idempotent.

Similarly, the mutating API, `DELETE`, only takes effect during the first call, returning a `200` ( `OK`) status, while subsequent calls return `404` (`Not Found`).

The last mutating API, `POST`, creates a new resource.

However, receiving multiple requests can cause servers to allocate different places for the new resource. Therefore, the `POST` API is usually **not** idempotent.
## **API gateways**

Although APIs offer security to the systems on either end, they are susceptible to overuse or abuse. If you're concerned about this, you may want to implement an _API gateway_ to collect requests and route them accordingly.

An API gateway is a reverse proxy that acts as a single entry point for microservices and back-end APIs.

![API Gateways](https://images.ctfassets.net/x78yjrjc11pq/4tHGzKCaieMmuTvVEoYSIl/f50a5868470011eb9ae8181375eaf89c/API_Gateways.png)

All client requests are routed to the gateway, which directs them accordingly—to the API if it is available, or redirected if the request fails to meet security standards.

Generally, use cases for API gateways include _user authentication_, _rate limiting_, and _billing_ if you're monetizing an API.

# **Learn more**[](https://www.aced.io/courses/system-design-interviews/system-design-apis#learn-more)

- **Netflix's Engineering Blog:** [Engineering Trade-Offs and The Netflix API Re-Architecture](https://netflixtechblog.com/engineering-trade-offs-and-the-netflix-api-re-architecture-64f122b277dd) and [Beyond REST](https://netflixtechblog.com/beyond-rest-1b76f7c20ef6) (learn about the engineering team's experiments with microservices over the years)
- [REST API response codes and error messages](https://www.ibm.com/docs/en/odm/8.8.1?topic=api-rest-response-codes-error-messages) (IBM)
- [Pagination vs. infinite scrolling vs. load more: Best practices on Oracle Help Center](https://crocoblock.com/blog/pagination-vs-infinite-scroll/)