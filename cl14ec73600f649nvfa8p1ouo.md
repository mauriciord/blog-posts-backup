# Web Development Fundamentals

## Client
A client is a process that basically requests data from a server. As we're talking about the web, the browsers are clients requesting HTML pages from servers.

## Server
Process providing service/data for a client

## Client / Server Model
Clients requesting data from servers
> Note: A piece of code/machine/software can be both server and client at the same time because it could act as a client requesting data from a database and serving a service as a server.

We've been used to associate Clients to Frontend and Servers to Backend

---

About the codebase executing on the client and the server:
### Client
- End-users can see all of the code (Browsers)
- Need to be fast
- Low computing power (depends on the end-user)
- Cannot access Database directly (you can store information in the Browser, but in this case, we need information/data shared across users)

### Server
- Hidden code and implementation details
- Slow (compared to clients because we need to wait on the network)
- Scalable computing power (💰💰💰💰💰)
- Can access Database easily and share information across users

