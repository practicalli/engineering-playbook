# Practicalli Makefile Tasks

Makefile tasks designed to support Practicalli software development and website development for Practicalli books.

[Practicalli Dotfiles](https://github.com/practicalli/dotfiles){target=_blank .md-button}


## Version Control

Manage multiple repositories using [:fontawesome-brands-github: mgitstatus](https://github.com/fboender/multi-git-status){target=_blank}.

`make git-sr` creates a situation report for all the local Git repositories in the current directory and sub-directories.

!!! EXAMPLE
    ```make
    # ------- Version Control ------------------------ #
    git-sr:  ## status list of git repos under current directory
    	$(info -- Multiple Git Repo Status --------------)
    	mgitstatus -e --flatten

    git-status:  ## status details of git repos under current directory
    	$(info -- Multiple Git Status -------------------)
    	mgitstatus
    # ------------------------------------------------ #
    ```


## Zensical

Makefile tasks to install Zensical using UV, including additional plugins such as zensical-catppuccin theme.

```make

# -- Makefile task config ------------------------ #
# .PHONY: ensures target used rather than matching file name
# https://makefiletutorial.com/#phony
.PHONY: all clean dist docs lint pre-commit-check
# ------------------------------------------------ #

# -- Makefile Variables -------------------------- #
DOCS_SERVER := zensical serve --dev-addr localhost:7777
# ------------------------------------------------ #

# --- Documentation Generation  ------------------ #
docs-install:  ## Install or upgrade Zensical with Catppuccin theme plugin
	uv tool install zensical --with catppuccin-zensical --upgrade

docs:  ## Build and run docs in local server
	$(info -- Local Server --------------------------)
	$(DOCS_SERVER)

docs-open:  ## Build docs, run server & open browser
	$(info -- Local Server & Browser ----------------)
	$(DOCS_SERVER) --open

docs-build:  ## Build docs locally
	$(info -- Build Docs Website --------------------)
	zensical build

docs-debug:  ## Run local server in debug mode
	$(info -- Local Server Debug --------------------)
	$(DOCS_SERVER) -v

mkdocs:  ## Build and run mkdocs in local server
	$(info --------- Mkdocs Local Server ---------)
	mkdocs serve --dev-addr localhost:7777

dist: docs-build ## Build mkdocs website
# ------------------------------------------------ #
```



## MkDocs with Python Virtual Environment

Install Material for MkDocs using Python Virtual Environment, including additional plugins that Practicalli commonly uses.

> NOTE: Practicalli is migrating to Zensical installed via UV.

```make
# -- Makefile Variables ---------------- #
MKDOCS_SERVER := mkdocs serve --dev-addr localhost:7777
# -------------------------------------- #


# -- Documentation Generation  --------- #
python-venv:  ## Create Python Virtual Environment
	$(info -- Create Python Virtual Environment -----)
	python3 -m venv ~/.local/venv

python-activate:  ## Activate Python Virtual Environment for MkDocs
	$(info -- Mkdocs Local Server -------------------)
	source ~/.local/venv/bin/activate

mkdocs-install:
	$(info -- Install Material for MkDocs -----------)
	source ~/.local/venv/bin/activate && pip install mkdocs-material mkdocs-callouts mkdocs-glightbox mkdocs-git-revision-date-localized-plugin mkdocs-redirects mkdocs-rss-plugin pillow cairosvg --upgrade

docs: ## Build and run mkdocs in local server (python venv)
	$(info -- MkDocs Local Server -------------------)
	source ~/.local/venv/bin/activate && $(MKDOCS_SERVER)

docs-changed:  ## Build only changed files and run mkdocs in local server (python venv)
	$(info -- Mkdocs Local Server -------------------)
	source ~/.local/venv/bin/activate && $(MKDOCS_SERVER) --dirtyreload

docs-build:  ## Build mkdocs (python venv)
	$(info -- Mkdocs Build Website ------------------)
	source ~/.local/venv/bin/activate && mkdocs build

docs-debug:  ## Run mkdocs local server in debug mode (python venv)
	$(info -- Mkdocs Local Server Debug -------------)
	. ~/.local/venv/bin/activate; $(MKDOCS_SERVER) -v

docs-staging:  ## Deploy to staging repository
	$(info -- Mkdocs Staging Deploy -----------------)
	source ~/.local/venv/bin/activate && mkdocs gh-deploy --force --no-history --config-file mkdocs-staging.yml
# -------------------------------------- #

```
