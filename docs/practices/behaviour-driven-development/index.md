# Behaviour Driven Development

![Behaviour Driven Development Workflow conceptually](https://raw.githubusercontent.com/practicalli/graphic-design/live/engineering-playbook/behaviour-driven-development-workflow-concept.png){align=right loading=lazy style="height:360px;width:360px"}

Behaviour-driven development (BDD) is an evolution of the ideas behind agile software delivery. With its roots in test-driven development, domain-driven design, and automated acceptance testing, BDD focuses on the ways an application is expected to work - its behaviour.

Understanding your domain and who your stakeholders are, identifying and exploring requirements, automating acceptance criteria, and delivering working and tested software.

BDD is writing software that matters

[Introducing BDD - Dan North](https://dannorth.net/introducing-bdd/){target=_blank .md-button}

## BDD concepts

| Concept              | Description                                                                                                                                                                                                             |
|----------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Outside-in           | understand system usage from the outside (user interfaces) inwards                                                                                                                                                      |
| Pull based system    | Produce only that which is ready to be used by people, systems, or code closer to a boundary or UI                                                                                                                      |
| Multiple stakeholder | Core and incidental stakeholders - Core stakeholders define the vision and often provide budget; Incidental stakeholders support the solution delivery and may influence that solution                                  |
| Multiple scale       | BDD applied from end to end, from small feature set to enterprise projects                                                                                                                                              |
| High automation      | Automation of testing (acceptance & unit) hence reducing the expense of scenario and examples testing; Enabling and inviting change through acceptance tests and reducing the impact of change                          |
| Features             | A slice through the whole solution; defined by multiple scenarios describing interactions stemming from user interface (outside-in). Features are defined with respect to business value                                |
| Feature Injection    | Only work on features that deliver value, according to the project vision/goals (in the context of the organisations goals)                                                                                             |
| Ubiquitous language  | Using the language of the domain consistently, end to end in the solution artefacts; name things with respect to their behavior in the domain. Using Given,When,Then also provides structure to the ubiquitous language |

![Behaviour Driven Development mind map](https://raw.githubusercontent.com/practicalli/graphic-design/live/engineering-playbook/behaviour-driven-development-mindmap.png)

## Example Exercise

This is a BDD style description of the case study requirements for the service called Boris Bikes.

The scenarios give you ideas of behaviour you should consider when trying to implement the case study in a Test Driven Development approach.

Pick a scenario and write one or more tests that will satisfy that scenario.

The Features here are not necessarily in line with the scenarios, please feel free to create new features that are more relevant.

!!! EXAMPLE "City Bike Hire Service"
    ```gerkin
    Feature: City Bikes Service
      In order to provide a convenient way to get around the City of London
      As the Mayor of a city
      I want to provide a bike hire service

      Scenario: scenario name
        Given .
        When .
        Then .


    Feature: Commute around the city quickly
      In order to get around London quickly
      As a commuter tired of using the tube (and tube strikes)
      I want to hire a bike to get me around central London


      Scenario: Hire a bike
        Given there is a bike available for hire
        When I identify myself to the bike hire station
        Then I get access to the bike

      Scenario: Return a bike
        Given I have taken a bike for hire
        When I identify myself to the bike hire station
        And return the bike correctly
        Then my bike hire ends
        And payment is taken

      Scenario: Find a parking slot
        Given I have taken a bike for hire
        And my current bike station has no free slots
        When I ask the hire station for available slots
        Then I get the number of slots for the 3 nearest bike stations

      Scenario: Register
        Given .
        When .
        Then .

      Scenario: Identify myself as a registered user
        Given I am a regular rider
        When I hire a bike
        Then I use my key at the bike station to identify myself

      Scenario: See if a particular bike station has bikes
        Given .
        When .
        Then .

      Scenario: Find nearest location with available bikes
        Given .
        When .
        Then .

      Scenario: Book a bike in advance
        Given .
        When .
        Then .
    ```

## References

[Introducing BDD - Dan North](https://dannorth.net/introducing-bdd/){target=_blank .md-button}
[Cucumber - Behaviour Driven Development](https://cucumber.io/docs/bdd/){target=_blank .md-button}
[Behaviour Driven Development - Wikipedia](https://en.wikipedia.org/wiki/Behavior-driven_development){target=_blank .md-button}
