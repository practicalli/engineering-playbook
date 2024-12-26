# Lua Rocks

Publish package to Lua Rocks

??? INFO "Lua Rocks API key required"
    Add Lua Rocks API to GitHub repository (or organisation) secrets

!!! EXAMPLE "GitHub Workflow to Publish package to Lua Rocks"
    ```yaml
    name: "release"
    on:
      push:
        tags:
          - 'v*'
    jobs:
      luarocks-upload:
        runs-on: ubuntu-22.04
        steps:
          - uses: actions/checkout@v3
          - name: LuaRocks Upload
            uses: nvim-neorocks/luarocks-tag-release@v4
            env:
              LUAROCKS_API_KEY: ${{ secrets.LUAROCKS_API_KEY }}
            with:
              detailed_description: |
                A meaingful description of the package that should show on Lua Rocks.
              dependencies: |
                plenary.nvim
    ```
