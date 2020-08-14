# Domain Driven Design - Notes

## Features

- Focus on business knowledge acquisition and translate it directly into software
- Strategic Design with Bounded Contexts and Ubiquitous Language

### Ubiquitous Language

-  Universal Language (Business Terms) which can be easily discussed with all team members within a team. Significant contribution required from Domain Experts.

### Bounded Context

- Boundary within which Ubiquitous language is relevant. e.g. 'Policy' model definition would differ between Underwriting/Inspections/Insurance bounded contexts

### Sub Domains 

- Divide up complex Domain in multiple Sub Domains to reduce complexity and deal with legacy systems

### Possible Architectures

- Event-Driven Architecture/Event Sourcing
- Command Query Responsibility Segregation (CQRS)
- Reactive And Actor Model
- RESTful
- MicroServices or SOA

### Context Mapping - Integration with other domains or sub domains

- Partnership - Common goals and supporting each other - co-ordinate releases together
- Shared Kernel - Single shared portion of the model (unusual)
- Customer-Supplier - Upstream/Downstream relationship
- Conformist - Upstream/Downstream where Downstream has a mandate (slave)
- Anti-corruption Layer - Upstream/Downstream where model differs and AntiCorrupitionLayer works as translation
- Open Host Service - Easy to translate
- Published Language - Upstream provides well defined document to consume structure for downstreams

### Approaches:

- Messaging: Command/Domain Events/Aggregates are published/subscribed via messages. At least once delivery from publisher for identpotent receiver
- RPC (Remote Procedure Call) with SOAP: WSDL describes the service; makes use of Service Registry to find service
- RESTful HTTP: POST;GET;PUT;DELETE

## Dive In!!

### Terms:
- Command - Causes effects on aggregates e.g. CreateProduct, ScheduleRelease
- Domain Event - State change of aggregate produces a fact that something happened e.g. ProductCreated, ReleaseScheduled
- Domain Events can be passed on to other subscribed bounded contexts for further effects
- Domain Events can be caused by non-command sources as well e.g. MarketClosed, FiscalYearEnded
- Domain Events can be saved in order to create Event log / Event database / Event store for previous state recreation

### Aggregates:

- Usually consists of: Root Entity, Value Objects, other Aggregate Roots 
- It is a transactional boundry - everything within scope of a single aggregate instance to be up to date and consistent within a single transaction. 
- If possible - Separate Transaction for separate aggregate.

### Four Rules for Aggregates:
  1. Protect business invariants inside aggregate boundries - usually separate transaction for separate aggregate but to protect business invariants do updates to multiple aggregates in single transaction
  2. Design small aggregates - large aggregates can cause transactional failures due to concurrent modifications ; avoid garbage collections
  3. Reference other aggregates by identity only - Smaller footprint
  4. Update other aggregates using eventual consistency - via domain events

### Modeling Steps:
  1. Design all aggregates with just 1 entity to start with unless you are sure
  2. Protect Business invariants - Aggregates which are dependent on each other
  3. Understand acceptable time frame for updates for each dependend e.g. immediate or eventually
  4. House all immediate components under one aggregate - for transactional consistency
  5. Private Setters instead of Public - only behavior is public
  6. Causal Consistency should be ensured - Order of events should be maintained for robust systems

### Event Storming:
  1. Model Domain Events - operations that happen within a business process. Model them in time order
  2. Model Commands that cause Events
  3. Model Aggregates and Entities
  4. Same Aggregate may receive a different command stimulus at different time - be aware
  5. Identify context boundaries - Identify Core Domain/Sub Domain/Different Bounded Content

### Credits
Notes taken during Domain Driven Design - Distilled Course by Vaughn Vernon
