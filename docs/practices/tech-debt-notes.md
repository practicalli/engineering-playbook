## Driving factors

Most technical debt has a root cause of time pressure or limited understanding.

### Time constraints

Rapid business changes increase presure on engineering teams to deliver, shift focus to delivering new feature with significantly reduced time to understand the impact of the changes.  Short term decisions can lead to extra work in the future when the wider picture is known.  

Engineers under increased pressure increasingly seek short-cuts to their work in order to keep up.

### Business or market change

A significant shift in business goals, customer needs, or market environment can significantly affect development work and render it obsolete. Significant technical debt up to entire systems may be required to keep pace and stay relevant when plans pivot.  This can create legacy systems that no one is able or is motivated to work on.

### Limited understanding or experience

A lack of understanding of the business domain or technology used to create the solution leads to less optomal solutions that require rework.  Limited understanding of the technology used to build solutions can lead to sub-optimal code and design, requiring rework and greater troubleshooting.

### Limited resources

Technical debt requires exponentially more time to manage and work around. Teams need to factor in technical debt to planning work, including time spent aleviating the debt so it does not grow to unsustainable levels.

### Limited sharing of knowledge

Code that is undocumented and created without thought of readability and maintenance increased the effort required to use that code and build new features upon it.

Issues such as regularly conflicting priorities, limited cross-team communication or poor quality are organisational debt, attributable by not investing time in how people can work effectively.

- limited understanding
- tools and workflows no longer fit for purpose
- technical architecutre constraints

most often incured by decisions made due to increased pressure the organisation faced at the time the decision was made.

Decisions made due to pressures on or from witin the organisation.  

Pressures people put on themselved, sometimes unreasonably so.

time pressures (assumed or actual) driving hurried decisions

changing situations (change in requirements, startup pivit, tooling issues, process, people)

communication challenges

## Aquiring debt

Technical debt is often driven by a need to balance business goals (and its customers) with the capability of the engineering teams to deliver (and remain productive in the longer term).  

Over a the longer term, Software entropy is another form of technical debt where an engineering team is inexperienced or unable to invest time in actively evolving the overall system design, leading to a deteriating product that requires an increasing effort to maintain.

In a broader sense, technical debt refers to many kinds of issues detrimental to effective delivery by technical teams, e.g. archtecutre, infrastructure, documentation, process, tooling, etc.  

Some technical debt issues are also driven by organisational debt. TODO: add reference

**Technical debt over time**

- design decisions early on lead to a system that can no longer scale or is otherwise limited
- inconsistent approach to code design and documentation limits the ability to extend and lenthens the time taken to maintain the system
- manually intensive opperations, e.g. running batch processes, data fixes, limit time available for new features

## Postive technical debt

Example:
Getting to market quickly with an essentail feature to start generating revenue.  Once revenue is generated then more time is available to create a maintainable system

Aleviate:
Capture important business and technical learnings from the project (during or straight after) reduces the rework to approximately a third of the effort, compared to starting again without oany prior knowledge.

Planning the delivery of a longer-term solution and understanding the constraints that drive the switch-over from the short-term tactical solution time

Context:
Common in early-stage startups where tactical solutions are used to discover a sustainable market or where a company is moving quickly into a new or quite different area of business.

> Also described as active or intentional technical debt

Making decisions without all the facts

- waiting for all the facts can miss valuable opportunities
- managing the level of risk around technical debt allows for experimentation and agility of the business without over-burdening the engineering teams ability to deliver

## Negative techical debt

Example:
writing code very quickly to satisfy unrealistic time pressures and creating code that is hard to work with and add more features. code tends to require significant rework (more than a simple refactor)

The debt is compounded if the learning when creating the quick solution is not captured and available for the rework of the solution.  

Without capturing the business & technical experiences learned, rework is essentially starting from the beginning and will require learning everything again.

Aleviate:
Ensure time is planned to capture important information regarding requirements and implementation

Context:

Chaning deadlines and requirements can hamper effective communication and remove essential time for considering the correct approach.  

- building the wrong thing due to limited or incorrect understanding of the requirement
- insufficient time to consider the most appropriate time

## Aquiring debt

## Retrospective assessment

When first starting to address technical debt

- journal tech debt as its is discovered and review at retrospectives to decide on actions
- capture potential issues in a document e.g. wiki (minimising interuption to flow)
- carve out time to expand on potential issues
- convert agreed upon as specific tickets, labelled as tech debt or part of an Epic style ticket to help understand the scale of debt

## Understand

## Managing

When reviewing a system, the principle of the technical debt is the cost of resolving all the idenfied technical debt

ensuring the constraints that drove the need for debt are understood is paramount to managing and alieviating the debt in a timely manor.

- acknowledgement by all that technical debt exists and must be effectively managed
- agree on what technical debt effects the team and plan to aleviate the debt with limited value or most detrimental effect
- balance feature delivery with the ability to effectively deliver features by understanding the wider value proposition of new features to the business and a realistic view of the impact of technical debt.
- review ways of work to limit inadvertant debt, e.g. cover working practices and challenges in a retrospective with engineering team and product manager

Specifically for the engineering team:

- automate tooling to support code quality and highlight potential technical debt, e.g. code analysis tools during development and automated quality checks in continuous integration

## Measuring

- Bugs: issues raised as bugs as a basic indicator of quality. An increase in long-standing bugs indicates a software system in increasing trouble
- Churn: amout of rework on code indicates a limited understanding of the domain or requirements changing rapidly
- cycle time: working on the same feature indicates a limited understanding of the requirement or rapidly changing requirements

## Aleviate

## Summary

"slow is smooth, smooth is fast" matra encouraged by the author, John Stevenson.

## Maing timely decisions

Decisions made now can adversly affect future productivity.
Making decisions too soon can be as impactful as making them too late

Decisions that bind the team or organisation down a particular route, which becomes increasing challinging and costly to deviate from.

Understanding the value of delivering features with respect to the timeframe in which those features are of value

e.g. supporting a sales & marketing team to roll out a campain in the run up to black friday.  If delivered after black friday, then of no value.

If delivered to quickly, decisions may have been unneccessarily rushed and proper levels of though not been part of the process, increasing the risk of mistakes and errors going live.

Understanding the value timeframe and the values within the campain ensures enough resources are dedicated to the campain in balance with the rest of the work required by the organisation

## What does the team assume is tech debt

## Understanding tech debt status

## Documentation

A lack of essential documentation increased the effort required to maintain and extend an existing codebase

- Define common practices in highly accessible and well structured documentation

## Tooling

Time should be factored in to ensure effecive use of the core development tools and delivery tool-chain (Continuous Integraion, Continuous Delivery, etc.)

Tools not fit for purpose

The alure and adoption of (shiny) tools without due dilligance - tools and workflows should be evaluated sufficiently to

## Team

## Technology

## Process

## Techniques to manage technical debit

Combatting technical debt due to communication and understanding

- 5 whys: ensuring a deeper understanding of a situation or requirement is atained

- 6 thinking hats: a critical thinking approach to discussion that seeks to create a rounded, positive and effective discussion on a particular subject.  Discussion is carried out with one focus at a time and with all participants aligned in their thinking approach at any one time.  

- MoSCow rules: priorities within the short term (day, week) to ensure essential features are delivered and critical issues are resolved (must have, should have, could have priority of tickets)

- rolling wave planning: ensures daily work is organised within a wider context of goals for the comming months

- value stream mapping: identify the critical path for delivering valuable product/service to the customer and use as a basis for optomising the delivery workflow (technical & non-technical)

## Case Study - Parrot cages

A person in the sales in the company tries to buy a parrot cage online and has a very unsuccessful experience, very limited choice and very expensive.  This raises the question, does everyone have this esperience and is there a viable business to be created.

The sales person works with engineer to build the essence of a website that will guage if there is interest in an online shop for parrot cages.  It is a simple html page with some CSS that shows parrot cages of different styles and costs.

Rather than build a full shopping cart experience, when customers press the buy button the website explains that the business is just starting and allows them to register interest via a form that captures contact details and preferences of bird cages.

When significant interest was measured, the app was redeveloped into a shopping cart application using external partners to manage payment, manufacturing and distribution of popular parrot cages.

## Case Study - Twitter technical stack

Twitter built their service using Ruby Rails architecture which enabled rapid development and architected to support the estimated user base.

Twitter quickly exceeded all expectations and the "fail whale" became a regular appearance on their website, indicating that the service was unavailable due to too many user.  The use of the service had also switched from a mobile driven service to heavy website use.

This is an example of success driving technical debt, because the technical architecture decisions were based on significantly inaccurate estimations and a different business model.

An extensive change to the technical architecture of twitter was required to meet the ever increasing capacity, which continued exponential growth.  However, more technical debt was created by working on the existing architecture in order to provide some level of service

The new architecure was built in parallel with the original service and was a significantly different approach that required new skills as well as technologies.

Too much success too quickly can cause technical debt.

Reference: <https://www.theatlantic.com/technology/archive/2015/01/the-story-behind-twitters-fail-whale/384313/>

## Case study - Rework a legacy system

A legacy system is running successfully in producition but is very costly to maintain and challenging to extend.

A large international finance organisation had a system that collected valuable business data from 20 different sources and distributed the data in near-realtime across 3 geographical regions.

The legacy system was able to distribute data, although had a significant scalability problem when data levels became too high.

The team who created and maintained the system were no longer at the organisation, all that remained was the undocumented codebase.

The core workflow of the system was assertained and the interaction from all the other applications was documented, to  understand the communication recieved.  This allowed a detailed application program interface (API) to be created and an appropriate solution created without concern of most of the inner workings of the legacy system.

Understanding the workflow and interaction with external systems allowed a new implementation of the system to be quickly created.  Tests discovered in the legacy system were applied to the new implementation to help verify the correct behaviour.

A testing environment was established and each development team responsible for an external system was invited and supported during the testing phase, to provide as close to live implementation testing as possible.

With an acceptance that techical debt may be neccessary at times,
