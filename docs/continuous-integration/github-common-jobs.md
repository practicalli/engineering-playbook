# Common GitHub Workflow Jobs

Common job configuration snippets taken from GitHub workflows, showing options typically used with GitHub actions.


## Git Checkout

Check-out the project from version control, fetching the whole history with `fetch-depth: 0`. Set `fetch-depth:` to 1 (or remove the option) to checkout the single commit for the ref/SHA that triggered the workflow

Echo the GitHub Repository that was cloned to the workflow log, to support debugging efforts.

      # Git Checkout
      - name: Checkout Code
        uses: actions/checkout@v3
        with:
          token: "${{ secrets.PAT || secrets.GITHUB_TOKEN }}"
          fetch-depth: 0   # fetch all history
      - run: echo "🐙 ${{ github.repository }} repository was cloned to the runner."
