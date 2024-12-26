# System catalog

## Backstage.io

[Backstage](https://github.com/backstage/backstage#what-is-backstage) is an open platform for building developer portals, using a centralised software catalog.

The Backstage Validator workflow checks the Backstage configuration of the current project.

!!! EXAMPLE "Validate Backstage.io configuration files"
    ```yaml
    ---
    # --- Validate Backstage.io configuration files ---#
    # https://github.com/marketplace/actions/backstage-entity-validator

    # trigger workflow if yaml files `.backstage/` directory updated
    name: Backstage Validator
    on:
      pull_request:
        paths:
          - ".backstage/"
    jobs:
      # Validate backstage configuration
      backstage-validator:
        runs-on: ubuntu-latest
        steps:
          - uses: actions/checkout@v3
          - uses: RoadieHQ/backstage-entity-validator@v0.3.2
            with:
              path: ".backstage/*.yaml"
    ```
