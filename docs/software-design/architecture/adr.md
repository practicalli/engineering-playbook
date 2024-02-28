# Architecture Decision Record

Architecture Decision Record (ADR) is a formal way to capture a significant decision which impacts the current or future system.

Everyone can learn from the history of desisions taken during the evolution of the project without adding burden to the current project team.

Once decisions are made and delivered the ADR document should be treated as immutable, so it becomes a perminant record.  Should a decision need to be changed or significantly altered, then a new ADR document should be created that references the orginal ADR.


## Scope

Create an ADR for architecturally significant decision that affects the software project or product

- Structure (patterns such as microservices)
- Non-functional requirements (security, high availability, and fault tolerance)
- Dependencies (coupling of components)
- Interfaces (APIs and published contracts)
- Construction techniques (libraries, frameworks, tools, and processes)

Functional and non-functional requirements are the most common inputs to the ADR process.


## Scenarios

ADRs work well in the following scenarios:

**Onboarding** 

New team members are able to find answers to why decisions were made in the project, allowing focus on new challenges whilst appreciating the existing approach.

Review ADRs when

- specific questions arise from learning a codebase
- when making related architectural decisions
- better understand how the team think and their priorities


**Ownership handover**: 

During a transfer of project the new maintenance team sees the resulting code, but that does not show how and why the project got to its current state. 

It is hard to know what are the critical paths of the project, the parts that are very robust and parts that should be treated delicately.

The new maintainers can 

- review past decisions to understand the current state. 
- avoid repeating the same discussion points 
- revisit topics in the future with knowledge of the historical context.
- review decisions quickly and effectively to understand priorities at the time they were made


**Alignment**: 

Teams can align on best practices across the organization when ADRs detail why certain decisions were made and alternatives were decided against.

> ADR's could also be used to capture decisions that form an aspect of the overall engineering strategy, e.g. choice of programming language


!!! EXAMPLE "Simple ADR template"
    ```markdown
    # Title

    ## Contributors
    - who owns the responsibility for the decision
    - contact details for all those involved (if the decision has follow-on or questions arise)

    ## Status
    
    Define the status of the decision along with the date of change into that state

    - proposed <date> 
    - accepted <date> 
    - rejected <date>  
    - deprecated  <date> 
    - superseded <date>

    ## Context

    - Primary Issue motivating the decision or change?
    - Business context & value
    - Timeframe for decision (usually tied to value)

    ## Decision

    Detail of the decision and any change to be made, including how that change is to be realised (i.e. 3rdc party product, bespoke development, migration, testing, deployment, etc.)

    Reference additional documentation already created or too be created as part of the change.

    ## Consequences

    Define how the decision impacts the project (and/or organisation), highlighing the benefits and constraints 

    - aleviated constraints
    - added constraints
    - intentional technical debt
    - business risks

    ## Alternatives

    Summary of the alternatives considered, incluing references to related documentation
    ```


## Backfill decisions

Decisions made before ADRs became part of the engineering workflow can be retrospectively created, although they may only be a partial record and contain historical inaccuracies regarding the original decsion.

Identify undocumented decisions during a retrospective or an architectural peer review, deciding which decisions are the most valuable to document. 


## References

[:globe_with_meridians: ADR management tools](https://github.com/npryce/adr-tools){target=_blank .md-button}

[:globe_with_meridians: Architectural Decision Records - Amazon](https://docs.aws.amazon.com/prescriptive-guidance/latest/architectural-decision-records/welcome.html){target=_blank .md-button}

[:globe_with_meridians: Example ADR - Amazon](https://docs.aws.amazon.com/prescriptive-guidance/latest/architectural-decision-records/appendix.html){target=_blank .md-button} 

[:fontawesome-brands-github: Architecture Decision Record Examples](https://github.com/joelparkerhenderson/architecture-decision-record){target=_blank .md-button} 
