# Version Control

Managing code changes is an important part of maintaining a usable code base and is the first step to having a robust development environment.

Versioning your code allows you to track all the changes that happen over the life of your software, giving you a useful audit trail of how your code has evolved as well as allowing you go back in history to earlier versions.

Projects are managed in a repository which provides

* An authoritative place to refer to all your project assets
* A complete history of the changes that happened during the project
* Ability to branch off and try alternative approaches / code solutions without polluting the main project (eg. sprints, experimental functionality)
* Collective ownership of the code as all developers take responsibility for the project assets

A repository can be central or decentralised


## Access remote repositories

Sharted Git repositories can be accessed via HTTPS or SSH URL.

SSH approach is typically more secure, especially as the files holding your keys on disk are encrypted.  SSH connections can be tunnelled through HTTPS if connecting to a remote repository via a very restricted firewall.

HTTPS connection requires a personal access token is required (GitHub blocks HTTP access via password).  A personal access token can be create with limited access, only allowing access to specific services and information. 

A personal access token is saved in a plain text file, e.g. `~/.github`.  Should a token be compromised, it does not give access to the account on the remote repository, so the token can be deleted and recreated without compromising the service login account.


## What to Version

Text based files such as source code, unit and acceptance tests, web pages, cascading stlye sheets, database scripts and configuration files should all be versioned to ensure that at any time you can build the whole project from the version control system.

Binary files such as images and logos should also be versioned, even though you cannot usually do meaningful diffs.  It is important to version those files that are part of the software project so your build, test and release scripts are always using the correct version.

It is important to also version your build files (build.xml, myproject.pom) so that you always have the correct script versions to build, test and deploy your project.  If all your assets are in the version control system, it make it very easy to set up and maintain a continuous integration server.

Compiled assets such as JavaDoc output, class files, jar and war files are not usually stored in the version control system as these can be generated.  This avoids duplication of information and ensures that information does not become outdated.



## Central or Decentralised Repositories



## Managing Repositories

Version control repositories can be managed centrally as with CVS and Subversion, meaning that there is a single server that holds all the repositories.

Repositories can also be decentralised as with Git, Mercurial and Bazaar, meaning repositories are held on many servers and developer computers that may be holding repositories.

Distributed repositories allow you the freedom to work on projects locally, making it easy to work in another direction or try out experimental code that may not be useful in the main project repository.

Managing changes (files vs change sets) - Branching and merging

Subversion likes to think about revisions. A revision is what the entire file system looked like at some particular point in time.

In a distributed system, you think about changesets. A changeset is a concise list of the changes between one revision and the next revision.

Assume that Bill and Ben are working on Project X, and both Bill and Ben branch Project X code.  Bill and Ben make their own changes in their separate workspaces, so they have diverged considerably.

If Bill and Ben decide to merge all these changes, Subversion tries to look at both revisions—Bill's modified code, and Ben's modified code—and it tries to guess how to join them together in back to one project.  Due to a lack of the complete history of changes then that merge is more likely to fail, producing many “merge conflicts” that aren’t really conflicts, simply places where Subversion failed to understand what has happened.

If Bill and Ben were instead using a distributed system (eg. Git), then Git would have kept a full history of the changes - the changesets for each of Bill and Ben. When the pair come to merge their code together, Git has the whole version history from the local repositories Bill and Ben were working with.  Those changesets can then be applied one by one and Git will find it much easie r to automatically merge them as it can see the full history.

For example, if you rewrite a method and then move that method to another class, Subversion wont necessarily relate those to actions, so when it comes time to merge, it might think that a new method was added. Whereas Mercurial will remember those things separately: method changed, method moved, which means that if you also changed that function a little bit, it is much more likely that Mercurial will successfully merge our changes.

Since distributed systems think of everything in terms of changesets, you get to do interesting things with those changesets. You can push them to a colleague on your team to try them out, instead of pushing them to the central repository and potentially causing problems for the whole project.


Keeping stable and dev code separate is precisely what source code control is supposed to let you do.


That means you can have team repositories, where a small team of programmers collaborates on a new feature, and when they’re done, they merge it into the main development repository, and it works!

That means you can have a QA repository that lets the QA team try out the code. If it works, the QA team can push it up to the central repository, meaning, the central repository always has solid, tested code. And it works!


## Concepts and Terms

### Repository
A repository is the magic box that contains all the information about your checked in items, the full history of your changes.

### Trunk
The trunk refers to the main set of files and changes that make up the project.    If you create a new repository and commit / push a change (eg. a new file) then you have added that change to the trunk.

A trunk is what you typically release and refer to as the main version of the project.

### Branch
A project under version control may be branched to work on new functionality or experimental development.  The branch becomes a separate independent copy, even though it is not actually be stored as a copy in the version control system.

Changes committed / pushed to the trunk after the branch are not applied to the branch and changes committed / pushed to the branch are not applied to the trunk.

### Tag
A tag or label refers to an important snapshot in time of your repository. The current version of all files can be tagged with a meaningful name or revision number. Tags are often used to indicate milestones or releases of a project.

### Head
The head is a special label that represents the latest version of your files in the repository.  When you checkout or clone a project you get the version that is pointed to by the head label - unless you specify a particular version.

### Commit
A commit (check-in) is the action of writing or merging the changes made in the working copy back to the repository. The terms 'commit' and 'check-in' can also used in noun form to describe the new revision that is created as a result of committing.

### Update
An update merges changes made in the central repository (by other people, for example) into the local working copy / local repository

### Clone
Creates a complete copy of a repository locally (used in distributed version control systems such as Git, Mercurial, Bazaar)


### Push
Merges change sets from your local repository to the main (parent) of your repository.

### Pull
Merge change sets from other repositories - you pull in change sets and merge them into your repository

### Promote
A more abstract term for committing or pushing, where changes are "promoted" from a less controlled location into a more controlled location. For example, from a user's workspace into a repository, or from a local clone to its main (parent).


### Diff
A diff (or delta) represents a specific change to a project under version control.  Diffs between different versions of files are created to see what changes have been between two versions and giving you information on how to merge two files together.


### Merge
A merge where two sets of changes are applied to a file or set of files. Some sample scenarios for merging include:

You update your working copy or local repository with changes made by other users, either from a central repository or from another repository.

You try to commit / push changes to files that have been updated by others since you checked out those files (or you last update).  The version control software automatically merges the files (typically, after prompting the user if it should proceed with the automatic merge, and in some cases only doing so if the merge can be clearly and reasonably resolved).

The project is branched before a problem that existed is fixed in the main trunk.  The fix is done in the trunk and then merged into the branch.

A branch is created, the code in the files is independently edited, and the updated branch is later incorporated into a single, unified trunk.


### Conflict
A conflict occurs when different changes are being applied to the same file, and the version control system is unable to reconcile the changes. The person making a merge needs to resolve the conflict by combining the changes manually, or by telling the version control system which version to use.


## Diff and Merge

When you want to merge code you may experience conflicts between your code and the code your merging.  If your version control tool is not able to automatically manage these conflicts, you will need to merge manually.

To see the differences between two code files you use a diff tool.  A diff tool will show text references or a visual representation of the differences.

Using the diff tool you can change your code so the conflicts are addressed and changes can be merged.


### Meld - visual diff and merge tool

Meld is a visual diff and merge tool. You can compare two or three files and edit them in place (diffs update dynamically). You can compare two or three folders and launch file comparisons. You can browse and view a working copy from popular version control systems such such as CVS, Subversion, Bazaar-ng and Mercurial.

Calling Meld from git-merge

```shell
git mergetool -t meld
```


## Git Advanced branching and merging

All of the changes that git was able to merge automatically are already added to the index file, so git diff shows only the conflicts. It uses an unusual syntax:

The commit which will be committed after we resolve this conflict will have two parents instead of the usual one: one parent will be HEAD, the tip of the current branch; the other will be the tip of the other branch, which is stored temporarily in MERGE_HEAD.

http://book.git-scm.com/5_advanced_branching_and_merging.html

