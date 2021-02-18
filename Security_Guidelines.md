# Design Guidelines for Secure Applications

This guide defines a set of application vulnerability categories to help you design and build secure applications and evaluate the security of existing applications.  

## Input Validation
Do not trust input; consider centralized input validation. 
Do not rely on client-side validation. 
Be careful with canonicalization issues. 
Constrain, reject, and sanitize input. 
Validate for type, length, format, and range.

## Authentication	
Partition site by anonymous, identified, and authenticated area. 
Use strong passwords. 
Support password expiration periods and 
account disablement. Do not store credentials (use one-way hashes with salt).
Encrypt communication channels to protect authentication tokens. 
Pass Forms authentication cookies only over HTTPS connections.

## Authorization	
Use least privileged accounts. Consider authorization granularity. 
Enforce separation of privileges. 
Restrict user access to system-level resources.

## Configuration Management	
Use least privileged process and service accounts. 
Do not store credentials in plaintext. 
Use strong authentication and authorization on administration interfaces. 
Secure the communication channel for remote administration. 
Avoid storing sensitive data in the Web space.

## Sensitive Data	
Avoid storing secrets. Encrypt sensitive data over the wire. 
Secure the communication channel. 
Provide strong access controls on sensitive data stores. 
Do not store sensitive data in persistent cookies. 
Do not pass sensitive data using the HTTP-GET protocol.

## Session Management	
Limit the session lifetime. 
Secure the channel. 
Encrypt the contents of authentication cookies. 
Protect session state from unauthorized access.

## Cryptography	
Do not develop your own. 
Use tried and tested platform features. 
Keep unencrypted data close to the algorithm. 
Use the right algorithm and key size. 
Avoid key management (use DPAPI). 
Cycle your keys periodically. 
Store keys in a restricted location.

## Parameter Manipulation	
Encrypt sensitive cookie state. Do not trust fields that the client can manipulate (query strings, form fields, cookies, or HTTP headers) 
Validate all values sent from the client.

## Exception Management	
Use structured exception handling. 
Do not reveal sensitive application implementation details. 
Do not log private data such as passwords. 
Consider a centralized exception management framework.

## Auditing and Logging	
Identify malicious behavior. 
Know what good traffic looks like. 
Audit and log activity through all of the application tiers. 
Secure access to log files. 
Back up and regularly analyze log files.


