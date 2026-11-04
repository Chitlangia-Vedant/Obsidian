Cryptography is the study of encrypting and decrypting information for secure transmission between two parties. Generally, there are two steps:

- **Encryption** is the process of converting plain text into code, or **ciphertext.**
- **Decryption** is the reverse - converting ciphertext back to **plaintext**.

# How it works
An encryption algorithm translates (encodes) plaintext into ciphertext, making it unreadable and therefore useless to anyone without the **decryption key**. There are two basic types of encryption.
## Symmetric encryption
Symmetric encryption uses the _same_ key for both encryption and decryption. It's fast and easy, provided that the key stays secret.

The strength of the key depends on:

- Its length
- Its entropy (or, the "randomness" inherent in creating it)
- How easy it is to deconstruct

Symmetric algorithms (or ciphers) take one of two forms: block or stream. **Block ciphers** encrypt data in blocks, where **stream ciphers** encrypt data one bit at a time. 

**AES**, or the **Advanced Encryption Standard** is a block cipher used across the internet. Key lengths range from 128 to 256-bits, which is quite long. AES using a 256-bit key is considered unbreakable.

## Asymmetric encryption
Asymmetric encryption uses two different keys for encryption and decryption. The encryption (public) key is visible to all, while the decryption key stays private. Asymmetric encryption takes longer, but it's considered to be more secure.

Some well-known examples of asymmetric encryption algorithms include **RSA**, which was developed alongside early **TLS (Transport Layer Security).** 

While RSA is considered less robust than **Diffie-Hellman Key Exchange**, Diffie-Hellman doesn't authenticate and RSA does.

Both use mathematics to generate decryption keys that are all but impossible for attackers to deconstruct.

# Symmetric and asymmetric encryption work together in SSL/TLS
Symmetric encryption is considered less secure than asymmetric, but it's faster and uses less computing power. To provide security over a network, these two methods are used in tandem.

**SSL (Secure Sockets Layer)** is an older form of **TLS (Transport Layer Security),** a protocol running on the application layer of the Internet that's designed to secure web communications through a **TLS handshake.** To establish a TLS connection, the client can either:

- Make a request using port 443 (reserved for encrypted HTTPS traffic), or
- Request that the server switch to TLS. If the server agrees, a handshake is initiated.

An asymmetric cipher generates a session-specific shared key while further communication is symmetrically encrypted. The overall connection is protected with the more secure asymmetric cipher, but beneath that, symmetric encryption is used to protect the data without slowing down transmission.

# Password protection with bcrypt
What about sensitive data stored in a database, like user passwords?

Generally, the best practice is to **hash** and **salt** passwords using a powerful, time-tested algorithm like **bcrypt.**

Hashing mathematically scrambles the data, making it difficult to reverse-engineer. But since passwords are _short_ and can be _predictable -_ like "MyPassword", hashed passwords may still be susceptible to brute-force attacks.

Salting helps address both of these by adding a unique value to the end of a password before hashing. This makes it longer and increases entropy.

# Further reading
Most big cloud providers use symmetric encryption at rest. Check out these links for details on how Microsoft Azure and Salesforce manage encryption.