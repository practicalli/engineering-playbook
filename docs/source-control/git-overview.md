# Git Overview

Git provides a decentralised approach to version control.

Versions are manage via a `.git` repository (clone) as well as central (master)



Can clone a clone
Can work disconnected from the central repository
Designed for branching and merging and long lived branches
Each change is a delta, strung together so merging can be done step by step (change sets)
Easy to take a repository, clone it to experiment with the code and then request to the original repository owner to merge your changes if they like what they see
Adopted by a very large number of open source projects, most new open source projects use git now.
Well supported by main developer IDEs (Netbeans built in, Eclipse)

Decentralised model
When ever you work with a project in a git repository, you take a clone copy.  A clone is a fully fledged Git repository in itself, with complete history and full revision tracking capabilities.

Having your own local git repository allows you to commit code as and when you like without affecting anyone else working on the project.  You are creating your own local changes which can be pushed back to the orignial project repository at a convenient opportunity (eg. when all your tests are passing)

Limitations?
Whole repository needs to be checked out - usually results in separating out code bases into their own repositories - aligns well with Maven modular project approach - change deltas stored very efficiently keeping size to minimum

Several new commands to learn if coming from a central repository approach, a small learning curve to understand the concepts.

Getting started with Git

Create git repository

Local repository and central repository (master)

Local Commit

Pushing to central repository

Merging (pulling in? ) changes from other repositories

Advanced code management

Operational Concerns
Backups
Security
