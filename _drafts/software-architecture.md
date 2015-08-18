## Software architecture

### Designing

Reducing the number of open options and making informed design decisions.

* requirements
  * functional requirements
  * non-functional requirements (quality attributes)
    * performance (response time, latency)
    * scalability
    * availability (eg. 99.9% uptime)
    * security
    * disaster recovery
    * accessability
    * monitoring
    * management
    * audit
    * flexibility (eg. ability to modify business rules by non-tech people)
    * extensibility
    * maintainibility
    * legal, regulatory and compliance
    * I18n
    * L10n
* constraints
    * time and budget
    * technology
        * approved technology lists
        * existing systems and interoperability
        * target deployment platform
        * technology maturity (eg. aloowed to use bleeding edge technology?)
        * open source
        * past failures
        * internal intellectual property
    * people
    * organisational
* principles
    * development
        * coding standards and conventions
        * automated unit testing
        * static analysis tools
        * etc
    * architecture
        * layering strategy
        * placement of business logic
        * high cohesion, low coupling, SOLID, etc (separation of concerns)
        * stateless components
        * stored procedures
        * domain model (rich vs anaemic)
        * use of HTTP session
        * always (immediately) consistent vs eventually consistent

Use MoSCoW for everything (must have, should have, could have, would/won't have).

### Visualising

Agility requires good communication.

C4
--

* context - the highest level of abstraction, represents something that delivers value to somebody (eg. financial risk management system, an Internet banking system, a website)
* containers - represents something in which components are executed or where data reside
* components - a logical grouping of one or more classes
* classes - the smallest building blocks

### Diagrams

* Context: A high-level diagram that sets the scene; including key system dependencies and actors.
* Container: A container diagram shows the high-level technology choices, how responsi-bilities are distributed across them and how the containers communicate.
* Component: For each container, a component diagram lets you see the key logical components and their relationships.

### Context diagram

* Users, actors, roles, personas, etc
* IT systems
* Interactions

Audience: tech and non-tech people.

### Container diagram

* Containers (eg. web servers, app servers, databases, other storages, etc)
  * for each: name, technology, responsibilities
* Interactions
* System boundary

Audience: tech people.

### Component diagram

* Components (eg. trade data system importer, risk calculator, auth service, notifiaction component, monitoring service, etc)
* Interactions

Audience: software development team.

### Effective sketches

* Titles
* Labels
* Shapes
* Responsibilities
* Lines (line style, eg. solid, dotted, dashed, etc; arrows; annotations)
* Colour
* Borders (e.g. double lines, coloured lines, dashed lines)
* Layout
* Orientation

### Documenting

* Context
* Functional Overview
* Quality Attributes
* Constraints
* Principles
* Software Architecture
* External Interfaces
* Code (eg. security, domain model, configuration, exceptions and logging, etc)
* Data
* Infrastructure Architecture
* Deployment
* Operation and Support (who will run, monitor and manage the software)
* Decision Log (why's)
* Risks (probability and impact)

### Nice reads

* Simon Brown "Software Architecture for Developers" https://leanpub.com/software-architecture-for-developers
* http://blog.8thlight.com/uncle-bob/2011/09/30/Screaming-Architecture.html
* https://speakerdeck.com/skwp/domain-driven-rails, http://www.windycityrails.org/videos/2014/#6
