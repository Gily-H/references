# Agile Development

__Principles of Agile__
- Individuals and interactions _over_ processes and tools
- Working software _over_ comprehensive documentation
- Customer collaboration _over_ contract negotiation
- Responding to change _over_ following a plan


## Properties of Agile Software Development

- Customer satisfaction drives development
	- frequent customer feedback
- Method understands change is inevitable and should be embraced
- Highly iterative - frequent incremental releases of the software
	- start with goals to solve customer needs
	- inspect work as it is being done
- Entire development process is transparent
	- Customers can see the progress of the project

|Processes and Methodologies|Techniques and Practices|
|:-|:-|
|- Planning| - Design|
|- Teamwork| - Coding|
|- Engaging Customers| - Testing|
|- Providing Leadership| - Deploying|
|- Collaboration| - User Experience|
|- Learning||


## Effective Requirements
A requirement is a feature, behavior or constraint that the customer wants to be added to the product. Requirements are what faciliates communication between the development team and the customer

- Independent - Requirements should be self-contained and executable
- Negotiable - Requirements are not contracts
- Valuable - Requirements must be useful to the customer, not necesarrily the developer
- Estimable - Requirements need to have an estimation time for betting planning
- Sized Appropriately - Requirements should be manageable - estimate half the time of a single iteration
- Testable - Must be able to demonstrate to the customer that the requirement was met

### User Stories
A User Story is a way to express a requirement in a more readable form

Format: `As a <role> I want <feature> so that <benefit>`

_Example #1:_ _As a_ premium member, _I want_ to cancel at the last minute with no penalty _so that_ I get a full refund
_Example #2:_ _As a_ non-premium member, _I want_ to cancel up to 24 hours in advance _so that_ I get a 50% refund
_Example #3:_ _As a_ site member, _I want_ an email confirmation of my cancelled reservation _so that_ I can have a record of the transaction


### Requirement Success Criteria
We can determine when a requirement is met by testing several scenarios (GIVEN / WHEN / THEN)

Format: `Given <context>[and <more context>], when <something happens> then <outcome>[and <another outcome>]`

_Example #1:_ _Given_ I am premium member, _when_ I cancel under 24 hours, _then_ I incur no penalty
_Example #2:_ _Given_ I am a non-premium member, _when_ I cancel less than 24 hours in advance, _then_ I pay a 50% fee
_Example #3:_ _Given_ I am a site member, _when_ I cancel my reservation, _then_ I am emailed a confirmation


## Estimating Work
Estimating time to complete a task is essential for proper planning. We can estimate work by using _Story Points_, which are based on size and complexity, and _not_ by duration. We compare the costs of performaing tasks and see if the next task is bigger or smaller than the previous task

There is no set units to measure Story Points, however, below are some examples of ways to measure Story Points
- Order of magnitude - powers of 2: [2, 4, 8, 16...]
- T-shirt sizing - [XS, S, M, L, XL...]


### Planning Poker
1. Each estimator has a deck of estimation cards
2. Customer reads a story and discusses the story briefly
3. Each estimator selects a card that will be their estimate and plays it face down
4. All cards are turned simultaneously
5. Discuss differences in estimates
6. Re-estimate until estimates converge


# Scrum
Scrum is one instance of a process that utilizes Agile methodologies

__Process__

|Product Backlog| -> |Spring Backlog| -> |2-4 week sprint| -> |Functional Product|

1. Highest priority item is taken from the Product Backlog
2. The item taken from the Product Backlog is broken into smaller tasks = Sprint Backlog
3. During the 2-4 week sprint, there are 24-hour iterations = progress updates on the Sprint Backlog


# Resources
Agile Manifesto - https://agilemanifesto.org/
Agile Alliance - https://www.agilealliance.org/
The Declaration for Interdependence
