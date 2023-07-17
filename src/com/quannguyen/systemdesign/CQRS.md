# CQRS
The **Command and Query Responsibility Segregation (CQRS)** is an architectural pattern where the main focus is to separate the way of reading and writing data. This pattern uses two separate models:

- **Queries** — Which are responsible for reading data
- **Commands** — Which are responsible for update data

![CSQR overview](/asset/image/systemdesign/CSQR/csqr_overview.jpeg?raw=true "CSQR overview")

## Glossary
### Commands
Commands represent the intention of **changing** the state of an entity. They execute operations like Insert, Update, Delete. Commands objects alter state and do not return data.

Commands represent a business operation and are always in the imperative tense, because they are always telling the application server to do something.

### Queries
Queries are used to **get data** from the database. Queries objects only return data and do not make any changes.

Queries will only contain the methods for getting data. They are used to read the data in the database to return the DTOs to the client, which will be displayed in the user interface.

### Event sourcing
Event Sourcing ensures that all changes to application state are stored as a **sequence of events**.

Not just can we query these events, we can also use the event log to reconstruct past states, and as a foundation to automatically adjust the state to cope with retroactive changes.

### Change Data Capture
Change Data Capture (CDC) is a solution that captures change events from a database **transaction log** (or equivalent mechanism) and **forwards** those events to downstream consumers.

CDC ultimately allows application state to be externalized and synchronized with external stores of data.

## CSQR usecases and attributes
### When to use CQRS
CQRS can be considered to be used on scenarios where:
- Has a high demand for data consumption
- Performance of data reads must be tuned separately from the performance of data writes, especially when the number of reads is much greater than the number of writes.
- There is a need for a team to focus on the complex domain model that is part of the write model, while another team can focus on the read model and user interface.
- The system is expected to evolve over time and might contain multiple versions of the model, or where business rules change regularly.
- Integration with other systems, especially in combination with event sourcing, where the temporal failure of one subsystem shouldn’t affect the availability of the others.
- You can afford stale data.

For short, CSQR is a good fit for big scalable system with complexity as its trade off.

### Advantages
- **Separation of concerns** — When the read and write sides are separate, the models become more maintainable and flexible. The read model is typically simpler, while the write model involves more complex business logic.
- **Independent scaling** — With CQRS, read and write workloads can scale independently, resulting in fewer lock contentions.
- **Optimized data schemas** — On the read side, a schema can be optimized for queries, and on the write side, a schema can be optimized for updates.
- **Security** — It’s easier to ensure that only the right domain entities are performing writes on the data.
- **Simpler queries** — Application queries can avoid complex joins by storing a materialized view in the read database.

### Considerations
- **Complexity** of the code and lead to a more complex application design especially when using messaging.
- **Messaging** — Although CQRS does not require messaging, it’s common to use messaging to process commands and publish update events. In that case, the application must handle message failures or duplicate messages.
- **Eventual consistency** — If you separate the read and write databases, the read data may be stale.

### Separated Databases

When working with CQRS, it’s also possible — but it’s not mandatory — to have separate databases:

- A **write database** only for making changes.
- A **read database** only for reading the data and creating materialized views.

A common implementation approach for separated databases is to use decoration pattern while propagating the change through workflows:
1. Customer interacts with system and make a change to **write database**. This process is the simplest in the call flow, and normally include one database.
2. **Decoration pattern**: The change is propagated through internal asynchronous processes (queues/pipelines). This could include various kinds of decorations:
    2.1. Acceptance/rejection rules applies.
    2.2. Throttling.
    2.3. Add more contexts and meta assets.
3. After being decorated, data could be propagated to (possbily more than one) **write databases**. A big system could have as many big **write databases** as the number of usecases.

Implementors need to make sure the eventual consistency of data between **databases**. Practically, we should think thoroughly from the beginning how the message propagation happens and track it properly. A backfill tool should be ready when needed.

### Separated Services / Models
As mentioned above, with decoration pattern, we can have as many services/models as the number of usecases. Thes could somehow link to the mental model of [Domain Driven Design](https://en.wikipedia.org/wiki/Domain-driven_design)

## Transactional Outbox Pattern
TODO - This is a section from [Medium article - CQRS Software Architecture Pattern: The Good, the Bad, and the Ugly](https://betterprogramming.pub/cqrs-software-architecture-pattern-the-good-the-bad-and-the-ugly-e9d6e7a34daf).

Reader could consider discover this to expand CSQR scope and related knowledge.

## References
- https://martinfowler.com/bliki/CQRS.html
- https://betterprogramming.pub/cqrs-software-architecture-pattern-the-good-the-bad-and-the-ugly-e9d6e7a34daf
- https://learn.microsoft.com/en-us/azure/architecture/patterns/cqrs
- https://microservices.io/patterns/data/cqrs.html