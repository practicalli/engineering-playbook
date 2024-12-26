# Technical Debt Overview

Technical debt occurs when balancing the needs of the business with the productivity of engineering teams. Choosing tactical solutions, proceeding with limited understanding and taking short-cuts are all generators of technical debt to varying degrees.

Intentionally acquiring technical debt can have significant business value if more can be achieved in a shorter time-frame, providing much needed revenue and time to implement a longer term solution.

Inadvertently acquiring debt beyond the capability of a team leads to spiralling debt, increasingly limiting the ability to get work done.

Unresolved debt leads to feature delivery delays, hidden risks, spiraling project costs, engineer burnout and eventually leaving in frustration.  

Engineers use the term technical debt to communicate tasks are needed to improving the current system, along-side tasks only focused on business features. Naturally a system upon which teams can deliver timely features has intrinsic value for the business and its customers.

As a metaphor, technical debt has grown to reflect any issue having a negative impact on the productivity of technical teams, e.g. infrastructure challenges, architectural decisions, design complexity, test coverage, documentation quality and development practices.

??? INFO "Technical Debt original definition - Ward Cunningham, 1992, OOPSLA Conference"
    Ward Cunningham suggested technical debt could be a benefit.  An engineer creates a quick solution with limited understanding of the problem, with the intention to rewrite the code once understanding had improved and alleviate the debt.

    The danger occurs when the debt is not repaid. Every minute spent on code that is not quite right for the programming task of the moment counts as interest on that debt. 

    Entire engineering organizations can be brought to a stand-still under the debt load of an unfactored implementation, object-oriented or otherwise.

??? INFO "Comparison to financial debt"
    A debt is a value gained that must be repaid along with the accumulated interest over the duration of the debt.  

    Aquiring debt can be a wise investment in the future or become an increasingly unsustainable cost.

    **Intentional Debt**
        A mortgate allowing the purchase of a house, repaying a large debt in managable amounts over an established time period

    **Inadvertant debt**
        Credit card debt where funds available for monthly repayments become insufficient to prevent spiraling interest growing the debt to unsustainable levels

## Debt scenarios

**Intentional technical debt**

The benefit out-ways the potential issues, which are resolved across the next releases

- exploring a new market, creating tactical (throw-away) solutions whilst retaining the learning required to implement a strategic longer-term solution (or develop a strategic solution in parallel).
- continuing with a releasing that delivers customer partial set of features, or has known issues, rather than delay the release.

**Inadvertent technical debt**

- hack together a solution to meet unrealistic deadline pressures.  No time is available to document lessons learned or plan time to mitigate the risks taken due to working on the next deadline.
- technical decisions made with limited understanding lead to a solutions that struggles to fit the long term strategy, but the system is so ingrained into development it is perceived as unchangeable.  Work continues to be hampered until ultimately a major redevelopment of the solution is agreed.

> [Death March (Edward Yourdon)](https://en.wikipedia.org/wiki/Death_march_(project_management)) is a term describing projects that continue to wallow in significant technical debt.

## Identifying debt

Time may be limited when first starting to address technical debt as time pressures are a major factor in creating technical debt.

Identifying debt and its risks can be time-consuming, so start by apply simple techniques

- journal technical debt as its is discovered during feature development
- use a shared document to capture potential issues e.g. wiki (minimise interruption to engineer flow)
- carve out time to expand on potential issues, individually and collectively as a team
- review at retrospectives to decide on actions (see [Managing debt](#managing-debt))
- convert agreed upon issues to actionable tickets, labelled as technical-debt
- create an Epic style ticket linking all technical debt issues to help understand the scale of debt and if the debt is growing

## Managing debt

When the driving factors of technical debt are understood, organisations can choose an appropriate level of technical debt and carefully manage it as part of the software development workflow.

Project management and engineering teams should agree on what constitutes technical debt and balance the value provided by the debt with its detrimental effects.  

Consider these aspects when reviewing the technical debt of a project:

- Type: intentional or inadvertent
- Impact: how is affecting the team, other systems and the business
- Amount: perception of how large the debt is
- Time-frame: duration of positive and negative effects
- Age: from active work or inherited legacy

Opportunities to alleviate technical debt should be included during planning work and experiences shared during retrospectives.

The collective positive and negative effects of technical debt are specific to an organisation, so should be agreed & understood across that organisation.

## References

The McKinsey survey shows it is common to have 20% of technical budgets diverted to resolving issues related to technical debt. [1]

  1. [Demystifying digital dark matter: A new standard to tame technical debt](https://www.mckinsey.com/capabilities/mckinsey-digital/our-insights/demystifying-digital-dark-matter-a-new-standard-to-tame-technical-debt)

[Wikipedia: Technical Debt](https://en.wikipedia.org/wiki/Technical_debt){target=_blank .md-button}
