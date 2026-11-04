- **Authentication** is the process of proving you are who you say you are when accessing an application.
- **Authorization** is the process of defining and enforcing access policies - that is, what you can do once you're authenticated.

# How it works
## Authentication (or AuthN)
The most common **authentication factors** are usernames and passwords. When the right credentials are given, a system considers the identity valid and grants access. This is known as **1FA** or **single-factor authentication**, and it's considered fairly insecure.

**Multi-factor authentication (MFA)** is a more secure alternative that requires users to prove their identity in multiple ways. This could be through:

- Single-use PIN numbers
- Authentication apps run by a secure 3rd party
- Biometrics

When choosing an authentication process, there are a few things to keep in mind.

First, remember that security measures shouldn't overly inconvenience users unless they're forced to use your service.

Second, there's an adage in cybersecurity that says "security is only as good as its weakest link." The weakest link is often users. Whether through password apathy, vulnerability to clever phishing schemes, or pure negligence, companies have their work cut out for them. Instituting policies that require good password hygiene can help (requiring long passwords with numbers and special characters, regular password updates, timed log-off, etc.)

**Auth0** is a well-known platform that sits between your application(s) and **Identity Providers** like Google or Facebook that offer **Single Sign-on (SSO),** connecting and securing you and your data through standard APIs, through a token-based authentication method called **JWT (JSON Web Token)** which is good for stateless applications (and protocols, like HTTP). Another method for authentication over the web uses sessions and cookies.

### Session vs. Token-Based Authentication
**Session-based authentication** (an older method) relies on the server to track authentication. When a user logs in to a website on a browser, the server creates a **session** for that user. A **session ID** is assigned and stored in a **cookie** in the user's browser, preserving authentication while the user is on the site. Typically, cookies are deleted when a user logs off, but some browsers use **session restoring**, which keeps the session cookies in memory even after the user logs off. Much easier than logging in each time you want to access a page.

**Token-based authentication** is different. When a user logs in, the server creates an encrypted token that allows users to perform any activity on the site. Instead of the server storing session IDs, the client stores the token, either in memory or in a cookie (much like session IDs.)

The main difference between token and traditional session-based authentication is that session-based authentication is very stateful. The server stores the session ID and user info in memory or in a separate session cache, which can become complex as the system or number of users scale.

Token-based authentication is not always stateless, though. Many API tokens are still stored in a database table so they can be verified or revoked.

JWTs are a particularly popular flavor of token-based authentication because they save user data inside the token and can be validated without the database lookups mentioned above. This makes them well-suited for serverless and stateless applications.

JWTs are:

- Small (they transmit quickly)
- Secure (they're asymmetrically encrypted)
- Widely-used (JSON objects are everywhere; virtually all programming languages have JSON parsers, and almost every big web API uses JSON)
- Transparent (their structure makes it easy to verify senders and that contents haven't been tampered with)

JWTs are not without their drawbacks. While they are encrypted, they cannot easily be revoked or invalidated on the server-side, introducing risk. Because of this, they typically use shorter expiration times. 

## Authorization (or AuthZ)
Once a user is authenticated, we still need to ensure that they're only allowed access to certain resources. Because unauthorized access to sensitive data can be so catastrophic, many companies set up access policies according to the **principle of least privilege**. That is, by default, you're only allowed access to what you absolutely need. A few common ways to segment access are:

- **Role-based (RBAC):** Users are assigned to a certain group (or role) that comes with set permissions. Some examples may include "admin", "member" or "owner."
- **Attribute-based (ABAC):** Users are permitted access according to attributes like title, certification, training, and/or environmental factors like location. Sometimes known as "policy-based access control" or PBAC.
- **Access Control Lists (ACL):** Each user or entity has individual permissions that can be turned on or off, similar to installing a new app on your phone and deciding which permissions to grant (location services, contacts, etc.)

ACL is typically used at a more granular level than either ABAC or RBAC - for example, to grant individual users access to a certain file. ABAC and RBAC are generally instituted as company-wide policies.

## OAuth 2
