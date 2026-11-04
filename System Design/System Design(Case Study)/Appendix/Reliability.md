A **reliable** system can _perform its function_, _tolerate errors,_ and _prevent unauthorized access or abuse._

Building for reliability means added security, error-handling, disaster recovery, and countless other contingencies.
# Retries

Under a **simple retry** strategy, an application detecting a failure will immediately retry. This might work well if the failure detected is unusual and unlikely to repeat, but for common transient failures (e.g. network failures) repeat retries may overload the downstream system once the network issue is resolved. **Delayed retry** holds the retry back for a set amount of time allowing the system to recover. Many engineers implement an **exponential backoff strategy** that systematically decreases the rate of re-transmission in search of an acceptable retry rate.

## Use cases

- Simple retry for unusual and transient errors. We recommend implementing some type of request limit to prevent overload.
- Delayed retries using exponential backoff for more common transient errors.

## Techniques & considerations

Retry buildup in high-traffic systems can lead to extremely high system load once the error is resolved. This is called the **thundering herd** problem, and it can cause even more problems than the transient error as your resource(s) struggle to cope with the request volume. A simple solution is to introduce **jitter**, or "randomness" to the delay intervals so that client requests don't synchronize.

From a UX perspective, keep in mind that in some cases it's better to fail fast and simply let users know. In this case, implement a low retry limit and alert users that they'll need to try again later.

# Circuit Breakers
While a retry pattern assumes that the operation will ultimately succeed, a **circuit breaker** accepts failure and stops the application from repeatedly trying to execute. This saves computing power and helps prevent the cascading failures we discussed above. Importantly, a circuit breaker has a built-in mechanism to test for whether a service has been restored unblock traffic accordingly.

For example, let's say an application sends a request.

All is working well and the circuit breaker is in a **closed state**. It monitors the number of recent failures, and if this number exceeds a threshold within a given interval, the circuit breaker is set to **open**. No further requests are let through. A timer is set, giving the system time to fix the issue without receiving new requests. Once the timer expires, the circuit breaker is set to **half-open**; A few requests are allowed through to test the system. If all are successful, the breaker reverts to its closed state and the failure counter is reset. If _any_ requests fail, the breaker reverts to open and the timer starts again.

## Use cases

- Prevents cascading failures when a shared resource goes down.
- Allows for a fast response in cases where performance and response time are critical (by immediately rejecting operations that are likely to timeout or fail).
- Circuit breakers' failure counters combined with event logs contain valuable data that can be used to identify failure-prone resources.

## Techniques & considerations

There are a few main points to remember when implementing circuit breakers.

- You'll need to address exceptions raised when a resource protected with a circuit breaker is unavailable. Common solutions are to temporarily switch to more basic functionality, try a different data source, or to alert the user.
- Configure the circuit breaker in a way that makes sense for 1) the recovery patterns you anticipate, and 2) your performance and response time requirements. Setting the timer correctly can be nuanced. It may take some trial and error.

# Saga
Saga is a strategy used most often in microservice architecture where completing an action (also known as a **distributed transaction**) entails successfully completing a set of **local transactions** across multiple independent services.

Imagine you run an ecommerce site. When a customer buys an item, their cart is updated, their payment is charged, the item is pulled from inventory, shipped, and invoiced. If you fail to pull the item from inventory, you'll need to reverse the charge to your customer - but without any structured **compensating transactions** in place to reverse the transactions _that have already_ s_ucceeded_, you're stuck.

A saga is an alternate structure. Instead of a single distributed transaction, the component local transactions are decoupled, grouped, and executed sequentially. Each string of related local transactions is a saga. If your saga coordinator (more on that below) detects a failure, it invokes a predefined set of "countermeasures", effectively reversing the actions already taken. In the case of our ecommerce site, the inventory number would revert back to its previous state, and payment and cart updates would be reversed.

To implement a saga strategy, you can either coordinate saga participants via **choreography**, a decentralized approach where each local transaction triggers others, or **orchestration**, where a centralized controller directs saga participants.

## Use cases

- Maintains data consistency across multiple services.
- Well-suited to any microservice application where **idempotency** (the ability to apply the same operation multiple times without changing the result beyond the first try) is important, like when charging credit cards.
- For microservices in which there aren't many participants or the set of counter transactions required is discrete and small consider choreographed saga as there's no single point of failure.
- For more complex microservices, consider orchestrated saga.

## Techniques & considerations

Sagas can introduce quite a lot of complexity into your system. You'll have to build the set(s) of compensating transactions that are triggered by different failures. Depending on your application, this might require substantial work upfront to understand user behavior and potential failure modes.