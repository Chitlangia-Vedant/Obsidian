**High availability (HA)** is difficult to achieve for two reasons.

1. Scaling, or building a system to accommodate changes in traffic, is hard.    
2. Networks and hardware will fail.

Availability is usually reported as a percentage of uptime / total time. It's impossible to guarantee 100% availability, but most cloud providers include a high availability guarantee — something approaching the highly-coveted 'five nines', or 99.999% availability — in their service level agreements (SLAs.)

# Rate Limiting
Rate limiting refers to capping the number of times an operation can occur within an interval. The main reason you'd want to impose rate limiting is to protect your service from overuse - intentional or not

It's important to note that you're not rejecting _all_ requests good and bad. You're simply capping usage by particular users, organizations, IP addresses, etc... sometimes even other parts of your system.

It's possible to enforce rate limits on both the client and server-side. Doing this will minimize latency, maximize throughput, and help manage traffic and avoid the excess cost.

## Use cases
- Prevent an autoscaling component from running over budget.
- Preserve the availability of a public API given a DoS attack or accidental overuse.
- Help SaaS providers control access and cost on a per-customer basis.

## Techniques & considerations

- **Token bucket:** A token bucket strategy recognizes that not all requests require equal "effort." When a request is made, the service attempts to withdraw a certain number of tokens in order to fulfill the request. A request that requires multiple operations may "cost" multiple tokens. If the bucket is empty, the service has reached its limit. Tokens are replenished at a fixed rate, allowing the service to recover.
- A **leaky bucket** works similarly. Requests are "held" in a bucket. Each new request fills the bucket higher and higher. When the bucket is full (and the limit has been reached), new requests are discarded, or leaked.
- **Fixed and sliding window:** Fixed window rate limiting is simplicity itself - for example, 1000 requests per 15-minute window. Spikes are possible, as there's no rule preventing all 1000 requests from coming in within the same 30 seconds. Sliding window "smooths" the interval. Instead of 1000 requests every 15 minutes, you might define a rule such as 15 requests _within the last_ 30 seconds