# Technical writing

Tools and writing techniques to deliver high quality documentation and articles on the topic of software development.


## A Practical approach

Write what you want to say, then rewrite the way you wish to say it, e.g brain-dump thoughts at first without concern for the prose, refactor to make thoughts intelligible and easily consumable.

The hardest part of writing can be getting over the blank page.

A focus on capturing thoughts first, a wider or deeper coverage of what is to be said. Once extensive thoughts are captured, apply refinement to how thoughts are express (without risk of loosing thoughts).


## Tool support

The [:fontawesome-brands-github: Proselint](https://github.com/amperser/proselint){target=_blank} tool improves the use of English, distilling decades of experience in writing modern English

Using Proselint within an editor provides feedback to improve writing as its written.

Add a `$XDG_CONFIG_HOME/proselint/config.json` file if there are checks that are not required or experiencing warnings that are not relevant, e.g. Python markdown syntax use for Zensical websites used to create the Practicalli content.


??? EXAMPLE "Practicalli dotfiles example Proselint config"
    A `proselint/config.json` configuration was created in [:fontawesome-brands-github: practicalli/dotfiles](https://github.com/practicalli/dotfiles){target=_blank} and a symbolic link to the proselink directory created in

    ```json
    {
      "checks": {
        "annotations": false,
        "lexical_illusions": false,
        "typography.diacritical_marks": false,
        "typography.symbols.curly_quotes": false,
        "typography.symbols.ellipsis": false,
        "typography.punctuation.hyperbole": false
      }
    }
    ```

    The reason for disabling these checks:

    - `annotations` warns about `TODO:` and similar notes in the text
    - `lexical_illusions` warn on repeated words (trips up on log and shell output examples)
    - `typography.diacritical_marks` warn on words containing marks affecting pronunciation
    - `typography.symbols.curly_quotes` warns double quotes in text should be curly - complains about code examples.
    - `typography.punctuation.hyperbole` false warning for annotation syntax
    - `typography.symbols.ellipsis` warns on `...


## Website generators

Static web sites are fast to serve and low maintenance, providing an excellent approach to serving up technical documentation.

Using Markdown or Asciidoc minimises the learning curve for creating documentation, allowing thoughts to be captured quickly without distracting concerns on visual presentation.


## Clojure tech docs

[cljdoc](https://cljdoc.org/){target=_blank} is a website that hosts documents for Clojure libraries and tools. Libraries pushed to [Clojars](https://clojars.org/){target=_blank} should have their docs available to build on cljdoc.org.

[cljdoc](https://cljdoc.org/){target=_blank .md-button}


## Tips

- learn [English grammar](https://en.wikipedia.org/wiki/English_grammar)
- avoid [English pronouns](https://en.wikipedia.org/wiki/English_pronouns), pronouns are often an indicator of verbosity
