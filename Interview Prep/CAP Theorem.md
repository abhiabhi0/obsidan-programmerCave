The CAP theorem says that a distributed system can deliver only two of three desired characteristics: _**consistency**, **availability**_ and _**partition tolerance**_ (the ‘**C**,’ ‘**A**’ and ‘**P**’ in CAP).

**Consistency**

Consistency means that all clients see the same data at the same time, no matter which node they connect to. For this to happen, whenever data is written to one node, it must be instantly forwarded or replicated to all the other nodes in the system before the write is deemed ‘successful.’

**Availability**

Availability means that any client making a request for data gets a response, even if one or more nodes are down. Another way to state this—all working nodes in the distributed system return a valid response for any request, without exception.

**Partition tolerance**

A partition is a communications break within a distributed system—a lost or temporarily delayed connection between two nodes. Partition tolerance means that the cluster must continue to work despite any number of communication breakdowns between nodes in the system.

For example, if you’re building a banking application, consistency is likely to be the most important property because you can’t afford to have different account balances for different users. On the other hand, if you’re building a social media application, availability is likely to be the most important property because users will expect the application to be up and running all the time.