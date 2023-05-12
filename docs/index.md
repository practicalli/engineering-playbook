# Practicalli Engineering Playbook

!!! QUOTE
    **Effective Communication** is the fundamental challenge for all software engineering projects

    John Stevenson, Practialli


The Playbook and Plays concept is used to curate a consistent information, so the right information can be found and used more effectively by all.

- A Play is a document that details practices and an approach for a specific task
- A Playbook is a collection of Plays that are related by topic, e.g. Billie Engineering playbook could cover the practices for deployment, accessing
databases, system integration testing, optimising builds in docker.

!!! HINT "Engineering Playbook Initiative"
    Establishing a playbook within an organisation encourages information sharing across teams and provides an effective way for disperate teams to learn from each other.

![Engineering Playbook concept](https://raw.githubusercontent.com/practicalli/graphic-design/live/engineering-playbook/engineering-playbook-concept.png)


??? WARNING "New Book - content under development"
    Practicalli Engineering Playbook was started in January 2022 in an attempt to codify the last few decades of development experience, so this will be an on-going work


## Development Workflow

Development workflow overview provides a high-level overview of the options available for developing services at Billie, including establishing a project, general development tools and continuous integration workflow.

Deployment workflow via Jenkins overview provides a high-level overview of the Jenkins CI pipeline, including how to Configure Service deployment on test instance via Jenkins Pipeline


## General Development tools

* MegaLinter - verify code and configuration consistency - a wide range of open source tools that run in a docker container to minimise install and as a GitHub action for CI support
* Makefile - consistent tasks across projects - simplify on-boarding and daily development of projects


## Programming Languages

Guides to development tools & workflows for specific programming languages, including testing and deployment of services

### Clojure

* Clojure Development Overview
* Clojure development environment guide
* Clojure Workflow - REPL Driven Development
* Deploy Clojure Services via Jenkins and System Integration testing

## Continuous Integration

* Dockerfile Multi-stage build configuration
* GitHub Actions and common workflow



## Book Source code

[practicalli/playbooks repository](https://github.com/practicalli/playbooks){target=_blank} contains the content for this book

=== "HTTPS"
    ```bash
    git clone https://github.com/practicalli/engineering-playbooks.git
    ```

=== "SSH"
    ```bash
    git clone git@github.com:practicalli/engineering-playbook.git
    ```


## Discussions and feedback

[Contributions are welcome via GitHub issues and pull requests](introduction/contributing.md), or discuss the book on the Clojurians Slack community.

[Ask questions on #practicalli channel of  Clojurians Slack](https://clojurians.slack.com/messages/practicalli){target=_blank .md-button}

Get a [free Clojurians slack community account](https://clojurians.net/)


## Sponsor my work

[![Sponsor practicalli-john](https://raw.githubusercontent.com/practicalli/graphic-design/live/buttons/practicalli-github-sponsors-button.png){ align=left loading=lazy }](https://github.com/sponsors/practicalli-john/)

The majority of my work is now focused on the [Practicalli series of books and videos](https://practical.li/){target=_blank} and an advisory role with several communities

Thank you to [Cognitect](https://www.cognitect.com/){target=_blank}, [Nubank](https://nubank.com.br/){target=_blank} and a wide range of other [sponsors](https://github.com/sponsors/practicalli-john#sponsors){target=_blank} from the Clojure community for your continued support


## Creative commons license

<div style="width:95%; margin:auto;">
<a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/"><img alt="Creative Commons License" style="border-width:0" src="https://i.creativecommons.org/l/by-sa/4.0/88x31.png" /></a>
This work is licensed under a Creative Commons Attribution 4.0 ShareAlike License (including images & stylesheets).
</div>
