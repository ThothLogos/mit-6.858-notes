# Lecture 3 - User Authentication

## Pre-Reading Material

 - https://techcommunity.microsoft.com/t5/azure-active-directory-identity/your-pa-word-doesn-t-matter/ba-p/731984
 - https://fidoalliance.org/specs/fido-u2f-v1.2-ps-20170411/fido-u2f-overview-v1.2-ps-20170411.html

## Lecture

User Authentication:

 - The foundation of many security policies
 - Interesting technical issues
 - Security vs Convenience

Standard Process:

 - Registration
 - Auth Check
 - Recovery

Challenges:

 - Intermediate principals (devices, load balancer, keylogger)
 - User Identity
   - Registration is generally a very weak form of identity
 - Human factor (weak passwords, re-used credentials)

Pro-active Password Implementation:

 - Use password only once per session - minimize exposure & swap to stronger secret (session key).
 - Use and encourage password manager - avoid weak & re-used passwords.
 - Rate limit password attempts to mitigate brute force
 - Augment passwords with 2FA

### Storing Passwords

Storing plaintext passwords is a terrible idea, so a method exists to avoid storing passwords:

![alt text](./imgs/0301_pwhash.png "Password Hashing")

Instead of storing the password itself, we use a one-way cryptographic hash function to produce
a unique hash of the password string, and store that. On the browser-side we can avoid ever sending
our plaintext password, by sending just the hash instead:

```javascript
sendCredentials(username, password, server) {
  let hash = sha256(password)
  server.login(username, hash)
}
```

This method, unfortunately, is vulnerable to Rainbow Table attacks. This is a pre-built database of
common/known passwords and their hashes. When brute-forcing, the attacker simply sends the hashes
and if one works for a login, they can derive the original password. A solution to this is to add
"salt" to the password before hashing.

Salt is just some random data appended to the input of the hash function, forcing the hash output to
be unique between services where the user may have used the same password. The same password, hashed
using different salt, will result in a different stored hash. Using this method we can thwart
Rainbow Table attacks.