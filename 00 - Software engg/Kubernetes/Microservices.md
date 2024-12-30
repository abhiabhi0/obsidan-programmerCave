[Microservices](https://www.atlassian.com/microservices) transform large software programs into smaller, more manageable services that communicate efficiently, enhancing operational efficiency and adaptability compared to traditional monolithic programs. They use a layered approach to security, from the source code to how they handle data and connections.

Microservices break large software programs into smaller, more manageable parts. These small parts, or services, communicate with each other in simple, efficient ways. Unlike the previous method of developing large, monolithic programs, this arrangement enhances operational efficiency and adaptability.

[Advantages of microservices](https://www.atlassian.com/microservices/cloud-computing/advantages-of-microservices) include ease of management, the ability to use different programming languages, and adaptability.

## Security
### Use authentication and authorization methods

When [building microservices](https://www.atlassian.com/microservices/microservices-architecture/building-microservices), it's essential to set up authentication and authorization. Authentication checks user identity. Authorization grants access rights to a user or machine.

### Control communication between microservices

Securing microservices with clear communication channels strengthens a network. Picture each microservice as a fort and their connections as bridges. Encrypted data travels across these bridges using transport layer security (TLS), which keeps private data safe.

Mutual TLS (mTLS) verifies the identity of both parties in a network connection and is vital to microservices security.

TLS and mTLS do more than keep data safe in an [Open DevOps](https://www.atlassian.com/solutions/devops) setting. They help build a network where every interaction between services is secure, which is essential for building a microservices system that's safe, reliable, and trustworthy.

### Implement centralized monitoring

Keep a close eye on your microservices by combining logging and monitoring. This will provide a clearer view of the system and help you locate security issues faster. Use monitoring tools like Prometheus for metrics collection and Grafana for visualization, or use comprehensive solutions like New Relic and Datadog to catch and address security concerns before they become critical.

### Advantages of microservices

- **Improved fault isolation**: Larger applications can remain mostly unaffected by the failure of a single module.
- **Eliminate vendor or technology lock-in**: Microservices provide the flexibility to try out a new technology stack on an individual service as needed. There won’t be as many dependency concerns and rolling back changes becomes much easier. With less code in play, there is more flexibility.
- **Ease of understanding:** With added simplicity, developers can better understand the functionality of a service.
- **Smaller and faster deployments**: Smaller codebases and scope = quicker deployments, which also allow you to start to explore the benefits of Continuous Deployment.****
- **Scalability**: Since your services are separate, you can more easily scale the most needed ones at the appropriate times, as opposed to the whole application. When done correctly, this can impact cost savings.

### Disadvantages of microservices

Microservices may be a hot trend, but the architecture does have drawbacks. In general, the main negative of microservices is the complexity that any distributed system has.

- **Communication between services is complex**: Since everything is now an independent service, you have to carefully handle requests traveling between your modules. In one such scenario, developers may be forced to write extra code to avoid disruption. Over time, complications will arise when remote calls experience latency.
- **More services equals more resources**: Multiple databases and transaction management can be painful.
- **Global testing is difficult**: Testing a microservices-based application can be cumbersome. In a monolithic approach, we would just need to launch our WAR on an application server and ensure its connectivity with the underlying database. With microservices, each dependent service needs to be confirmed before testing can occur.
- **Debugging problems can be harder**: Each service has its own set of logs to go through. Log, logs, and more logs.
- **Deployment challengers**: The product may need coordination among multiple services, which may not be as straightforward as deploying a WAR in a container.
- **Large vs small product companies**: Microservices are great for large companies, but can be slower to implement and too complicated for small companies who need to create and iterate quickly, and don’t want to get bogged down in complex orchestration.