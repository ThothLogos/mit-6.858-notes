# Lecture 3 - User Authentication

## Pre-Reading Material

 - https://techcommunity.microsoft.com/t5/azure-active-directory-identity/your-pa-word-doesn-t-matter/ba-p/731984
 - https://fidoalliance.org/specs/fido-u2f-v1.2-ps-20170411/fido-u2f-overview-v1.2-ps-20170411.html

## Lecture

__User Authentication:__

 - The foundation of many security policies
 - Interesting technical issues
 - Security vs Convenience

__Standard Process:__

 - Registration
 - Auth Check
 - Recovery

__Challenges:__

 - Intermediate principals (devices, load balancer, keylogger)
 - User Identity
   - Registration is generally a very weak form of identity
 - Human factor (weak passwords, re-used credentials)

__Pro-active Password Implementation:__

 - Use password only once per session - minimize exposure & swap to stronger secret (session key)
 - Use and encourage password manager - avoid weak & re-used passwords
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

This method, unfortunately, is vulnerable to __Rainbow Table__ attacks. This is a pre-built database of
common/known passwords and their hashes. When brute-forcing, the attacker simply sends the hashes
and if one works for a login, they can derive the original password. A solution to this is to add
__salt__ to the password before hashing.

Salt is just some random data appended to the input of the hash function, forcing the hash output to
be unique between services where the user may have used the same password. The same password, hashed
using different salt, will result in a different stored hash. Using this method we can thwart
Rainbow Table attacks.

### Two-Factor Authentication (2FA)

Used in conjunction with passwords to strengeth the user-auth process. 2FA helps defend against weak
passwords and phishing attacks. It requires the attacker to compromise two unique systems to get
access to a single service. Additional factors can be added to a __Multi-Factored Authentication__ (MFA)
approach.

__Approaches:__

 - Additional login "code" step
   - SMS (weak to SIM attacks)
   - Authy (potentially weak through backup option)
   - Google Auth (no backups or transfer available)
 -  