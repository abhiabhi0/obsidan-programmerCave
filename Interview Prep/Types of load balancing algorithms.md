A [load balancer](https://www.cloudflare.com/load-balancing/) is a software or hardware device that keeps any one server from becoming overloaded. A load balancing algorithm is the logic that a load balancer uses to distribute network traffic between servers (an algorithm is a set of predefined rules).

#### Dynamic load balancing algorithms

- _Least connection:_ Checks which servers have the fewest connections open at the time and sends traffic to those servers. This assumes all connections require roughly equal processing power.
- _Weighted least connection:_ Gives administrators the ability to assign different weights to each server, assuming that some servers can handle more connections than others.
- _Weighted response time:_ Averages the response time of each server, and combines that with the number of connections each server has open to determine where to send traffic. By sending traffic to the servers with the quickest response time, the algorithm ensures faster service for users.
- _Resource-based:_ Distributes load based on what resources each server has available at the time. Specialized software (called an "agent") running on each server measures that server's available CPU and memory, and the load balancer queries the agent before distributing traffic to that server.

#### Static load balancing algorithms

- _Round robin:_ [Round robin load balancing](https://www.cloudflare.com/learning/dns/glossary/round-robin-dns/) distributes traffic to a list of servers in rotation using the [Domain Name System (DNS)](https://www.cloudflare.com/learning/dns/what-is-dns/). An authoritative nameserver will have a list of different [A records](https://www.cloudflare.com/learning/dns/dns-records/dns-a-record/) for a domain and provides a different one in response to each DNS query.
- _Weighted round robin:_ Allows an administrator to assign different weights to each server. Servers deemed able to handle more traffic will receive slightly more. Weighting can be configured within [DNS records](https://www.cloudflare.com/learning/dns/dns-records/).
- _IP hash:_ Combines incoming traffic's source and destination [IP addresses](https://www.cloudflare.com/learning/dns/glossary/what-is-my-ip-address/) and uses a mathematical function to convert it into a hash. Based on the hash, the connection is assigned to a specific server.