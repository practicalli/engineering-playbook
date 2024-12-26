# Value Stream Mapping

- identify route cause of waste, to reduce or eliminating

- improve behavior, culture, communication, and collaboration

Teams discard individual opinions and prioritize towards a customer delivery perspective.

Cross team software development processes or where teams depend on each other

## Identify waste

waste can appear between

- the business / product owners and the engineering team
- operations delay developers (infrastructure limitations or issues can delay deployment)
- development teams delay operations by not providing details on how to support deployed systems
- support teams unable to support a system due to limited or no documentation
- customer facing teams unable to support customers effectively as documentation or updates were not communicated effectively

Value stream mapping can be used to improve any process where there are repeatable steps – and especially when there are multiple handoffs.

Waste in knowledge work occurs very often in handoffs or wait time between team members, not within the steps themselves.

<!--
Inefficient handoffs lead to low productivity and poor quality. Value stream mapping helps identify waste and streamline the production process. Value stream mapping can be applied to both the product and customer delivery flows. Product flow focuses on steps required to optimize product delivery and completion. The customer flow focuses on the steps required to deliver on end user requests and expectations.
->

Value stream mapping (VSM) is a lean manufacturing technique to optomise the flow of materials and information required to bring a product to a customer.

Valu stream mapping can be also be used to optomise software development and continuous delivery workflows.

## Effective approach

Balance the cost of conducting value stream mapping with the potential for value (reducing waste)

Appreciate the value particular activities and ensure that percieved waste is actual waste.

- involve people experienced with the process, espcially where the mapping process is cross-functional and complex

- avoid over-identifying waste (waste obsession)

- small savings may not directly translate to significant process improvements or be immediately obvious (ensure to record potential savings no matter how small)

- start with simple tools to focus on the activity

## Symptoms

### Over-production
- deliverying features not required
- delivering features an the incorrect time,to late or too early
  - delivering features too early may have prevented other features being delivered on time

Inventory
- maintaining features that are of little or no value

Defects
- high level of bugs or low quality software
  - rushed delivery, limited understanding, lack of support

Over-processing
- an overburdend test suite that only delivers partial value
  - code coverage is only a number and should only be a simple indicator of the value of tests
  - unit tests should include the public API of the system and only essential supporting code where testing provides significant value

Waiting
-

Transport
- waste when delivering products to the customer,

Partially completed work
- software delivered in an incomplete state
  - lack of complete specification or automated test coverage
  - can cause a cascade of waste fro additional work required to push more updates and provide missing features

Delays
- breaking changes to dependant systems causing delays
  - overly coupled system or system integration increases potential issues
  - non-breaking (additive) changes avoid delays and waste
- workflow delays
  - unit testing that takes a long time, limiting feedback and making it less likely that unit tests are run

### Task switching

Creative thought context switching is expensive. There is a cadence or “flow” that software engineers achieve to optimally produce good code.

Efficient organizations work to optimize the creative state for their engineers. Inefficient organizations bombard their engineers with non-critical distractions like meetings and emails that disrupt their workflow.

> Mute Slack channels that are not high priority to avoid being distracted by chat, reviewing interesting channels at a time that does not distract

> Limit @ mentions and encourage most of the team to switch off from slack when focusing deeply on work

- switching prioritories and focus too often
  - creative thought is very Inefficient when regularly task switching
    - context is disrupted
    - context takes time to be reestablished, a few minutes interuption can waste 30-60 minutes
- meetings disrupt productivity (organise meeting free days, or one day a week for meetings, e.g. Monday used for planning and meetings, leaving the rest of the week free for productive work)
  - limiting opportunities for meetings encourages more effecive communication via meetings and limits waste within meetings - as does a stand-up meeting as people dont want to stand for too long, forcing the meeting to be quicker)

Task switching (within a person) waste has similar qualities as handoff waste (beteween people)

Defects
Defect waste happens when bugs are pushed in software. Defects are similar to partially completed work but can be more wasteful because defects are unknown and partially completed work is usually known ahead of time. Defects may be identified by customers and then reported to customer support, which can be an expensive pipeline that causes delays and task switching.

- [Wikipedia: Value-stream mapping](https://en.wikipedia.org/wiki/Value-stream_mapping)
- [Atlassian: Value Stream mapping](https://www.atlassian.com/continuous-delivery/principles/value-stream-mapping) - optomise the continuous-delivery pipeline

## Waste

Examples of percieved waste that is not wasteful

- pair / mob programming is not a waste of resources when valuable learning and experiences are shared
- building parallel solutions is not wasteful when understanding is gained as to the most relevant architecture and designs to persue
