# Structurizr - Architecture diagrams as code

[Structurizr](https://structurizr.com/dsl) is a tool for expressing and visualising architecture using the [C4 model](https://c4model.com/)

Define a single **model** divided into **softwareSystems** within which **services** and **persistent data stores** are defined. **Relationships** are defined between services and persistence stores.

Many views can be generated from the single model and changes to the model automatically update those views to ensure all views are always up to date.

![Mock Fintech Startup - Fraud service deployment view](https://raw.githubusercontent.com/practicalli/graphic-design/live/architecture/structurizr-fintech-aws-deployment-fraud.png "")

> Colours of exported SVG image enhanced using [Inkscape.org](https://inkscape.org) and exported as PNG file

[Mock Fintech Starup - Practicalli Services](https://structurizr.com/share/79474){target=_blank .md-button}
[practicalli/structurizr Git repository](https://github.com/practicalli/structurizr){target=_blank .md-button}


## C4 Model summary

* Level 1: **Software system** - a system context composed of one or more containers
* Level 2: **Container** - application or data store, each component is separately deployable/runable, composed of one or more components
* Level 3: **Component** - grouping of functionality with a well defined interface, implemented by one or more code artefacts
* Level 4: **Code** - code artefact (function, object/class)

![c4 model abstraction](https://raw.githubusercontent.com/practicalli/graphic-design/570b6f55d83294a732c1e6b1c8d822d6ab2ab9ac/architecture/c4-model-structure-concept.png)


!!! HINT "Quick try with Structurizr DSL Editor"
    [Structurizr DSL online editor](https://structurizr.com/dsl){target=_blank} provides an instant way to try structurizr without install or sign-up


## Install

Use the free Structurizr Lite locally (via docker) or use Structurizr Cloud service (free for Open Source & Academic projects on request)

Project models can be imported into the cloud-based tool from Structurizr Lite when team or company wide collaboration is required.


=== "Structurizr lite"
    [Structurizr Lite - Getting Started](https://structurizr.com/share/76352/documentation#getting-started){target=_blank .md-button}

    Install Structurizr locally with the free [structurizr Lite](https://structurizr.com/help/lite) product via the Docker image. Ensure Docker or Docker Desktop is running  (image approximately 450Mb in size)


    === "Docker Compose"
        Include Structurizr Lite in a `docker-compose.yaml` file in the root of the project.  Docker compose is especially useful when running one or more services to automatically update views from the model.

        A volume is use to persist the model `workspace.dsl` file, enabling local editing of the model while the docker container is running.
        ```docker title="docker-compose.yaml"
        ---
        version: "3.9"

        services:
          # --- System Model --- #
          structurizr:
            container_name: system-architecture
            image: "structurizr/lite:latest"
            ports:
              - "8080:8080"
            volumes:
              - "./model:/usr/local/structurizr:rw"
        ```

        Run structurizr lite using the command:
        ```
        docker compose up
        ```

        Structurizr can also be defined in its own docker compose file and called separately, e.g. `strucurizr.yaml` configuration file
        ```shell
        docker compose -f structurizr.yaml up
        ```


    === "Docker"
        Pull the docker image

        ```shell
        docker pull structurizr/lite
        ```

        Set the Structurizr project path to define the location of the `.dsl` or `.json` workspace definitions (or enter the path directly in the docker command)

        Practicalli uses the `model` directory to keep the configuration files

        ```
        export STRUCTURIZR_DATA_PATH=/home/practicalli/projects/practicalli/structurizr/model
        ```

        Run docker with the Structurizr data path
        ```
        docker run -it --rm -p 8080:8080 -v $STRUCTURIZR_DATA_PATH:/usr/local/structurizr structurizr/lite
        ```



     ![Structurizr Docker run output](https://raw.githubusercontent.com/practicalli/graphic-design/live/architecture/structurizr-docker-run-output-dark.png#only-dark){loading=lazy}
     ![Structurizr Docker run output](https://raw.githubusercontent.com/practicalli/graphic-design/live/architecture/structurizr-docker-run-output-light.png#only-light){loading=lazy}

     `workspace.dsl` and `workspace.json` files are created if they do not already exist.


=== "Structurizr Cloud"
    [Structurizr Cloud](https://structurizr.com/){target=_blank .md-button}

    Sign up for a free account which provides 1 workspace on the cloud service.

    ??? INFO "Free Cloud product for Open Source & Academic use"
        Access to 5 workspaces on the Structurizr paid cloud service for each [open source project](https://structurizr.com/help/open-source) or [academic     establishment](https://structurizr.com/help/academic) on request

    Select New workspace after login to [Structurizr](https://structurizr.com/)

    A summary page of the new workspace is shown, showing the last modified time and an option to load a previous version.

    ![Structurizr new workspace summary page](https://raw.githubusercontent.com/practicalli/graphic-design/live/architecture/structurizr-workspace-summary.png)

    Scroll the page vertically to see the available diagrams, or click **More Diagrams...** at the bottom of the page

    ## Structurizr DSL editor


    The model is defined by a domain specific language (DSL) and the DSL editor should be used to add and update all configuration.

    Select DSL editor from the left hand navigation bar

    Structurizr DSL Language Reference documentation

    Select the Source text icon to see only the code window, allowing for easier editing


### Define the system

[Structurizr DSL Language Reference documentation](https://github.com/structurizr/dsl/blob/master/docs/language-reference.md){target=_blank .md-button}
[Structurizr DSL Cookbook](https://github.com/structurizr/dsl/blob/master/docs/cookbook/README.md){target=_blank .md-button}
[Structurizr DSL online editor](https://structurizr.com/dsl){target=_blank .md-button}

The [Structurizr DSL online editor](https://structurizr.com/dsl){target=_blank} is a useful tool to help learn the syntax of the Structurizr DSL, using the **Render** button to see the results of the model as it is defined.

Basic structure:

* `workspace` is composed of the system model and views derived from that model
* `model` is composed of one or more `softwaresystems`
* `views` define one or more views of the model, using autoLayout to organise diagrams or manually specifying layouts, sytlyes, themes, etc.
* `softwaresystem` defines specific services along container boundaries (application * data store components)

Add an entry for each service within the specific `softwareSystem` using the form

```
unique_name_id = container "Service or data store name" "Description of service or database" "Container name" "View type - Tag name"
```

??? INFO "Tags and themes"
    Tags are used to define the appearance in a view.  Structurizr default theme contains simple tags.  Amazon AWS theme contains a wide range of icons and styles to represent its many services, although these are more suitable for deploymentEnvironment views.  [Adding themes section](#themes) covers this in more detail.

For a persistent store, e.g. relational database, use the form

```
fraud_data = container "Fraud History" "A complete history of transactions and reports" "fraud-data" "database"
```

For an elastic search service, use the form

```
unique_name_id = container "Display name" "Description of service or database" "Elastic Search" "Elastic"
```

simple example

* Create a model with a user and a software system, where the user uses the software system.
* Create a system context view for the software system, adding the default set of elements, using auto-layout.
* Use the default theme for styling elements and relationships.


```
workspace {
    model {
        user = person "User"
        softwareSystem = softwareSystem "Software System"

        user -> softwareSystem "Uses"
    }
    views {
        systemContext softwareSystem "Diagram1" {
            include *
            autoLayout
        }
        theme default
    }
}
```


<!--
> Billie example: https://gist.github.com/practicalli-john/662b232b1c0ee119d1686b61f6104bd8
-->


### Common values

```
!constant ORGANISATION_NAME "Practicalli"
!constant GROUP_NAME "Fintech"

workspace {
    model {
        enterprise "${ORGANISATION_NAME} - ${GROUP_NAME}" {
            user = person "User"
        }
    }
}
```


### Grouping services

Grouping services within a softwareSystem keeps closely related service together and are rendered in the group within a view. A group can
also be included or excluded from a view

The Banking software system contains groups: shared, credit, transaction and fraud. Each group containing a number of services

```
risk = softwareSystem "Risk" {
  shared_services_risk = group "Shared Services Risk" {
    company_info = container "Company WhoIs" "Company search service" "Clojure API"
    company_info_database = container "Company WhoIs database" "" "Relational database schema" "Database"
    company_info_search = container "Company Search Aggregator" "Company full-text search index" "Elastic Search" "Elastic"
    risk_data_providers = container "Risk Data Providers" "Data Provider Service" "PHP Symphony service"
    risk_data_providers_database = container "Risk Data" "" "Relational database schema" "Database"
  }
  credit_risk = group "Credit Risk" {
    score = container "Credit risk scoring" "Scoring organizations Credit Risk" "Clojure Service"
    score_data = container "Credit Risk Scoring Service database" "" "Relational database schema" "DatabaseWip"
    assessment = container "Credit Assessment" "Credit risk assessment Service" "Clojure"
  }
  transaction = group "Transaction" {
    guardian = container "Transaction Guardian" "Transaction monitoring and transaction Screening service" "Clojure"
    guardian_database = container "Transaction Guardian Database" "" "Relational database schema" "Database"
    limiter = container "Limiter" "Limits Service" "ClojureKafka"
  }
  fraud_risk = group "Fraud Risk" {
    detection = container "Fraud Service" "Detect fraudulent transactions via Fraud Scoring Data Science models " "Clojure API"
    detection_data = container "Fraud Database" "TODO: Define the kind of data persisted" "Relational database schema" "Database"
    ml_model = container "Machine learning model service" "Sagemaker"
    feature_store_data = container "Feature store" "Pre-calculated feature values" "Key value database" "Database"
    manual_review = container "Review Transactions" "Manually review transactions for fraud" "" "WebBrowser"
  }
}
```


### Define relationships

`->` defines a relationships between two id's defined in the `softwareSystem` part of the model, along with a description of the relationship that is added to the arrow joining the artefacts in a view.

```
service_name -> service_or_datastore_name "Description of connection"
```

The relationships are used to draw connections between services and the descriptions name those connections

```
    user -> transaction "Triggers"

    transaction -> risk "Uses"
    guardian -> guardian_data "Persists"
    guardian -> limiter "Uses"

    detection -> detection_data "Reads and writes to"
    detection -> ml_model "score transaction"
    ml_model -> feature_store_data "Collect features"
    ml_model -> feature_schema_data "Request feature set & model"
```


## Defining Views

A workspace contains views which visualise artefacts defined in any `softwareSystem` within the model.

A view can include everything * within the softwareSystem or use `include` and `exclude` to refine the view based on groups or specific containers.

View of the form: view-type softwareSystem-name view-name

`container risk fraud-detection`

```
container risk "RiskContainersAfterSE" {
include *
autoLayout
}
container risk "CreditRiskServices" {
include *
exclude fraud_risk
autoLayout
}
container risk "FraudServices" {
include *
exclude credit_risk
autoLayout
```

## Deployment Infrastructure

An example of production deployment environment for the Practicall Mock Fintech Startup

```
  production = deploymentEnvironment "Production" {
    aws = deploymentNode "Amazon Web Services" "" "" "Amazon Web Services - Cloud" {
      region = deploymentNode "US-East-1" "" "" "Amazon Web Services - Region" {
        route53 = infrastructureNode "Route 53" "" "" "Amazon Web Services - Route 53"
        elb = infrastructureNode "Elastic Load Balancer" "" "" "Amazon Web Services - Elastic Load Balancing"
        autoscalingGroup = deploymentNode "Autoscaling group" "" "" "Amazon Web Services - Auto Scaling" {
          ec2 = deploymentNode "Amazon EC2" "" "" "Amazon Web Services - EC2" {

            webApplicationInstance = containerInstance detection
            elb -> webApplicationInstance "Forwards requests to" "HTTPS"
          }
        }
        rds = deploymentNode "Amazon RDS" "" "" "Amazon Web Services - RDS" {
          mysql = deploymentNode "MySQL" "" "" "Amazon Web Services - RDS MySQL instance" {
            databaseInstance = containerInstance detection_data
          }
        }
        route53 -> elb "Forward requests to" "HTTPS"
      }
    }
  }
```



## Embedding Documentaion in views

Create a directory called `docs` to contain markdown files with system descriptions.

Include `![](embed:DiagramName)` in the markdown file to include the text in the view called `DiagramName`

```markdown
## Context

Here is a description of my software system...

![](embed:Diagram1)
```

The view should be defined in the workspaces.dsl

```
views {
        systemContext softwareSystem "DiagramName" {
            include *
            autoLayout
        }
```

??? EXAMPLE "Example views from Mock Fintech Startup"
    Views defined in the Practicalli Enterpirse for the Mock Fintech Startup architecture
    ```
      views {
       /* Overall system */
        systemContext risk "EnterpriseView" "Practicalli Enterprise Application" {
          include *
          autoLayout
        }
       /* Entire Risk system */
        container risk riskView "Complete Risk system" {
          include *
          autoLayout
        }
        /* View of shared_services group in risk system */
        container risk sharedServicesView "Services shared across the organisation" {
          include shared_services
          autoLayout
        }
        /* View of fraud_risk group in risk system */
        container risk fraudRiskView "Fraud Risk Services Only" {
          include fraud
          autoLayout
        }
        /* View of credit_risk group in risk system */
        container risk creditRiskView "Credit Risk Services only" {
          include credit
          autoLayout
        }
        /* View of fraud & shared_services group without credit */
        container risk fraudSharedView "Fraud and shared services" {
          include *
          exclude credit
          autoLayout
        }
        container transaction transactionView "Current Transaction system" {
          include *
          autoLayout
        }

        deployment risk "Production" "AmazonWebServicesDeployment" {
          include *
          autolayout lr
          animation {
            route53
            elb
            autoscalingGroup
            webApplicationInstance
            databaseInstance
          }
        }

        /* Theme for views */
        themes default https://static.structurizr.com/themes/amazon-web-services-2022.04.30/theme.json https://raw.githubusercontent.com/practicalli/structurizr/main/themes/practicalli/theme.json

        branding {
          logo https://raw.githubusercontent.com/practicalli/graphic-design/live/logos/practicalli-logo.png
        }
      }
    ```


## Adding themes

Themes add styles or icons to the diagrams rendered by Structurizr tools, e.g. Amazon AWS icons.

Add a theme to the workspace.dsl configuration and add a theme tag to a component definition.

`theme` key is added as a value within the `views` key of the `workspace`.  `themes` is used to include multiple themes.

> [Structurizr language reference - theme](https://github.com/structurizr/dsl/blob/master/docs/language-reference.md#theme)

Example: using the Amazon icons

```
    views {
        systemContext softwareSystem "Diagram1" {
            include *
            autoLayout
        }
        theme https://static.structurizr.com/themes/amazon-web-services-2022.04.30/theme.json
    }
```

Use icons from the theme by adding one of the theme tags

```
identifier = type "" "" "" "tag-name"
```

Exmaple: the `"Amazon Web Services - SageMaker"` tag is added to a machine learning model

```
ml_model = container "AWS Sagemaker" "Machine learning model service" "" "Amazon Web Services - SageMaker"
```

??? HINT "Useful Amazon Web Services Tags"
    [Amazon Web Services theme](https://structurizr.com/help/theme?url=https://static.structurizr.com/themes/amazon-web-services-2022.04.30/theme.json) - tags commonly used:

    * Amazon Web Services - Lambda
    * Amazon Web Services - Fargate
    * Amazon Web Services - Route 53
    * Amazon Web Services - Simple Storage Service
    * Amazon Web Services - EC2
    * Amazon Web Services - EC2 AMI
    * Amazon Web Services - EC2 Auto Scaling
    * Amazon Web Services - Elastic Load Balancing
    * Amazon Web Services - Elastic Container Service
    * Amazon Web Services - Elastic Container Kubernetes
    * Amazon Web Services - Elastic Container Registry
    * Amazon Web Services - Elastic Container Registry Image
    * Amazon Web Services - EKS Cloud
    * Amazon Web Services - Elastic Beanstalk
    * Amazon Web Services - Category Database
    * Amazon Web Services - Category Machine Learning
    * Amazon Web Services - Category Serverless
    * Amazon Web Services - Corretto
    * Amazon Web Services - Data Pipeline
    * Amazon Web Services - Deep Learning Containers
    * Amazon Web Services - RDS
    * Amazon Web Services - DocumentDB
    * Amazon Web Services - DynamoDB
    * Amazon Web Services - Managed Service for Grafana
    * Amazon Web Services - Managed Service for Prometheus
    * Amazon Web Services - Managed Streaming for Apache Kafka
    * Amazon Web Services - Managed Workflows for Apache Airflow
    * Amazon Web Services - Secrets Manager
    * Amazon Web Services - Single Sign On
    * Amazon Web Services - Virtual Private Cloud

## Custom Theme

Create a theme as a `.json` file that containes a collection of definitions within `"elements": [ ]`

Each element should define the `"tag"` name used in the view definition to identify the type of element to use.

The look of the element is defined by `"colour"`, `"background"`, `"stroke"` hex color values and a `"shape"` type, e.g. `"RoundedBox"`, `"Cylinder"`

Include an `"icon"` as a further visual representation of the element, e.g. using AWS theme icons.

[Element style DSL reference](https://github.com/structurizr/dsl/blob/master/docs/language-reference.md#element-style){target=_blank .md-button}
[Practicalli Structurizr theme](https://github.com/practicalli/structurizr/blob/main/themes/practicalli/theme.json){target=_blank .md-button}

??? EXAMPLE "Practicalli Custom theme and AWS theme"
    ```structurizr title="practicalli/structurizr theme/practicalli/theme.json
    {
      "name" : "Practicalli theme",
      "elements" : [ {
        "tag" : "Clojure Service",
        "background" : "#8FB5FE",
        "color" : "#EEECE6",
        "stroke" : "#5881D8",
        "shape" : "RoundedBox",
        "icon" : "https://raw.githubusercontent.com/practicalli/graphic-design/live/logos/clojure-logo-64.png"
      } , {
        "tag" : "Database",
        "background" : "#EEECE6",
        "color" : "#3f51d4",
        "stroke" : "#3f51d4",
        "shape" : "Cylinder",
        "icon" : "https://static.structurizr.com/themes/amazon-web-services-2022.04.30/Arch_Amazon-RDS_48.png"
      } , {
        "tag" : "Web Browser",
        "shape" : "WebBrowser"
      } , {
        "tag" : "AWS SageMaker",
        "background" : "#EEECE6",
        "stroke" : "#2d8f7a",
        "color" : "#2d8f7a",
        "icon" : "https://static.structurizr.com/themes/amazon-web-services-2022.04.30/Arch_Amazon-SageMaker_48.png"
      } ]
    }
    ```

[Practicalli Structurizr theme](https://github.com/practicalli/structurizr/blob/main/themes/practicalli/theme.json){target=_blank} provides `Clojure Service`, `Database`, `Web Browser` and `AWS SageMaker` tags to customise the views generated.

The `Database` tag uses the icon from the AWS theme, although changes colors and shape to improve readability of the diagrams. The `AWS SageMaker` tag overrides that provided by AWS theme, again improving the readability of the diagrams.

The Mock Fintect Startup example uses a custom theme by Practicalli and the standard AWS theme.  It also includes the Practicalli Logo that appears next to the name of the view in each diagram

```structurizr title="practicalli/structurizr model/workspace.dsl extract"
themes default https://static.structurizr.com/themes/amazon-web-services-2022.04.30/theme.json https://raw.githubusercontent.com/practicalli/structurizr/main/themes/practicalli/theme.json

branding {
  logo https://raw.githubusercontent.com/practicalli/graphic-design/live/logos/practicalli-logo.png
}
    ```


## Organisation branding

Add a PNG or Jpeg graphic as a logo on all diagrams, appearing in the bottom left of each page

Define a specific font to use for all text in the digram (font downloaded from Google Fonts)

Define a `branding` section in the **workspace** > **views** section of the `workspace.dsl` file.

```
branding {
    logo <file|url>
    font <name> [url]
}
```

Practicalli uses the following branding in the structurizr `workspace.dsl`

```structurizr title="practicalli/structurizr model/workspace.dsl extract"
branding {
  logo https://raw.githubusercontent.com/practicalli/graphic-design/live/logos/practicalli-logo.png
}
```


## Comments

`/* */` for line comments.

```
/* single-line comment */
/*
    multi-line comment
*/
```

> Note: line comments within parens or directly after a closing paren cause syntax error


## Practicalli System

The [Practicalli Mock Fintech Startup](https://github.com/practicalli/structurizr/blob/main/themes/practicalli/theme.json) is defined as workspace that contains a model with an enterprise "Practicalli" key defining the high-level software systems

* Credit Assesment
* Risk Analysis
* Transactions (credit card, National bank deposit/transfer, BACS)
* Shared Services (account management, etc)

A simple mock up of a Fintech managing transactions that may be susceptible to various risks, including fraud

[Practicalli Structurizr project - Mock Fintech Startup](https://github.com/practicalli/structurizr){target=_blank .md-button}

`workspace.dsl` defined a Practicalli Enterprise with several softwareSystem defintions, each representing aspects of the business

Each softwareSystem is composed of containers that represent a logical service and related containers are grouped together

Relationships between containers are defined, stating the direction and relationship name, represented as arrows in the model views

`workspace.dsl` also contains a deploymentEnvironment for production, defining the infrastructure that containers are deployed into

A range of views are defined, using include and exclude options to refine the containers that are shown

Practicalli Structurizr custom theme and AWS theme are included, along with Practicalli logo in the branding



## Resources

* [Diagrams as Code 2.0](https://dev.to/simonbrown/diagrams-as-code-2-0-82k) - Simon Brown, 2021.
* [Getting started with Structurizr Lite](https://dev.to/simonbrown/getting-started-with-structurizr-lite-27d0) - Simon Brown, 2021
* [Structurizr DSL: AWS Example deployment](https://structurizr.com/dsl?example=amazon-web-services)
* [Structurizr DSL: Big Bank](https://structurizr.com/dsl?example=big-bank-plc)
* [Structurizr/dsl code examples](https://github.com/structurizr/dsl/tree/master/src/test/dsl)
* [Model example: Amazon Web Services local](https://github.com/structurizr/dsl/blob/master/src/test/dsl/amazon-web-services-local.dsl)
