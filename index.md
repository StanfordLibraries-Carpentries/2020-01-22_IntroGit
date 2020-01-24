# Notes from 'Intro to Git and Github' (January 2020)

Based on the Software Carpentry Lesson '[Version Control with Git](https://swcarpentry.github.io/git-novice)' and additional suggestions from Byron J. Smith.

Author: João Rodrigues (j.p.g.l.m.rodrigues@gmail.com)



## Intro

* Git is a tool for version control.
* Track Changes for Word is a form of version control (draft_v64_2.docx).
* Records the content and authors of changes to a document but does not record history (no undo)
* Why should you care? Simple way to keep a lab notebook in the digital era - history, changes, accountability, backups.
* Version Control is used by SEs to facilitate collaboration.
* Main features include rewinding changes, merge multiple contributions, solve conflicts.

## Automated Version Control with Git
* git is a free, _distributed_ version control system: everyone owns a copy of the project.
* Git does not store whole documents, only _changes_: very efficient storage and fine control.
* Terminology:
  * A collection of files and their recorded changes is called a repository.
  * Each change recorded into history is called a commit.

## Setting up Git
* git bash/git MacOS
* Subcommands: `git <verb>`
* Get help with: `git config -h`
* `git config --global user.name "Joao Rodrigues"`
* `git config --global user.email "...@gmail.com"` -> Email will be public!
* `git config --global color.ui "auto"`
* `git config --global core.editor "nano -w"`
* If stuck, undo: `git config --global --unset http.proxy`
* MacOS/Linux: `git config --global core.autocrlf input`
* Windows: `git config --global core.autocrlf true`

## Creating a Repository
* `cd Desktop & mkdir guac-joao`
* `git init`
* `ls -a` shows `.git`
* `git status`

## Keeping Track of Changes
* Explain staging area, or index: comparable to framing a scene before you take a photo.
* Diagram: [file] (add)-> [staging area/index] (commit)-> [commit history]
* `nano recipe.txt`
* "Chop avocados, squeeze limes, mix well"
* `git status`
* `git add recipe.txt`
* `git commit`
* Explain importance of good commit messages
* `git status`
* `git log`
* **Exercise**
  * Add a new line with *"Add salt"* to the file and commit the changes.
  * Use `git commit -m` short format

---

* Create a new file called `ingredients.txt`
* Add *cilantro* to the recipe and the list of ingredients.
* Add/Commit both files at once.
* Change last message, useful for typos: `git commit --amend`

---

* Add a new line: `add salt`

* Review changes: `git status && git diff`

* How to read a diff:

  ```bash
  # Which files are we comparing
  diff --git a/interfacea/core/__init__.py b/interfacea/core/__init__.py
  # hash of pre-change, post-change, and file mode
  index e69de29..564e7c4 100644
  # Source/Target of changes. Usually same. /dev/null if file is new.
  --- a/interfacea/core/__init__.py
  +++ b/interfacea/core/__init__.py
  # List of hunks of differences.
  # From line X, Y lines, for each file.
  @@ -0,0 +1 @@
  # +/- to represent addition or change/removal of lines to file.
  +# HELLO
  ```

* Add to staging area:`git add`

* `git diff` shows nothing. It compares current version to index version.

* `git diff --staged` shows differences. It compares index version to last commit.

* **Exercise**

  * Add `onions` to `recipe.txt` and `ingredients.txt`
  * Explore differences. Can you isolate diff to one file only?
  * `git diff recipe.txt`

## Git meets GitHub

* Git repositories are local (.git folder).
* GitHub is a web service that hosts repositories, designed for collaboration.
* Create account on GitHub
* Create a new github repo `guac-joao`
* On our computer, 'connect' local repo to online repo: `git remote <name> <url>`
* Explain concept of remotes
* `git push origin master` - explain origin/master
* Update diagram with final layer -> [Github]
* Show GitHub UI.



---

### Break

---



## Exploring History

* Git records a history with _all_ changes.
* This history allows us to recover and review old versions.
* The most recent commit is called _HEAD_
* `git show HEAD` shows the message and diff of the last commit.
* Append `~N` where N is a number bigger than 0 shows the N-th previous commit.
* `git show HEAD~1`
* Highlight multiple repo versions [HEAD] [HEAD~1] on diagram
* `git diff` can also be used to compare specific files to their previous versions.
* `git diff HEAD recipe.txt`  Why no output?
* `git diff HEAD~1 recipe.txt`  Works.
* Compare to specific version using commit hashes:
* `git log`
* `git diff <hash> recipe.txt`
* Show diff/history tab on GitHub.

## Reverting Changes

* We can not only see previous versions, but we can restore them too.
* 3 points in time where a change can be reversed: not staged, staged, committed.
* Add *orange* to `ingredients.txt`
* `git status`
* Revert changes before staging: 
  * `git checkout -- ingredients.txt`
  * `cat ingredients.txt`
* Reinforce the idea that small changes are better. Easier to revert!
* Repeat the same non-sense
* Reverting staged changes: 
  * `git checkout HEAD ingredients.txt`
  * `cat ingredients.txt`
* If your forget the file name, you end up in a detached head state: `git checkout master` to revert
* Repeat again the same non-sense
* Reverting committed changes:
  * `git checkout <hash> ingredients.txt`
  * `git revert ingredients.txt`
* **Exercise**
  * `rm *.txt`
  * Can we revert this change?
* **Question:** Which commit hash should I use to revert the third last change?

## Ignoring Things
* Sometimes we don't want to keep track of some files (intermediate analysis, compilation artifacts, etc)
* `touch recipe_mom.draft recipe_dad.draft`: start a new recipe
* `git status` shows the file as untracked.
* `nano .gitignore` : special file that allows us to tell git what files to ignore.
* `git status` does not show the file anymore.
* We must commit the `.gitignore` file to the repository too!
  * `git add .gitignore`
  * `git commit -m "Added ignore rules for drafts"`
* `git status`
* .gitignore has a specific syntax/rules. Order also matters (2nd rule can overrule the 1st).
* `nano recipe_grandma.draft`: this one is a draft but we want to keep!
* `!*grandma*` anything that grandma does is worth saving!
* Alternatively:
  * `git add -f recipe_grandma.draft`
* `git status`
* `git push origin master`
* **Question**:  `*.data && !*.data` What does it do?



---

### Break

---



## Collaboration!

* Pair Students. One of you will be the 'owner', the other the 'collaborator'.
* Owner: add collaborator's name to *Settings/Collaborators* tab on GitHub.
* Collaborator: 
  * Clone the owner's repository (take link from Github):
    * `git clone <repo URL> <name>-guac`
  * Make a small change to one of the files:
    * `nano recipe.txt` add an arbitrary ingredient.
  * Push your changes back to the Owner's repository:
    * `git push origin master`
* Owner:
  * Pull the new changes to your local repository:
    * `git pull origin master`
* Now switch roles and repeat.

## Solving Conflicts

* What happens when both people edit the same file at the same time?
* Owner:
  * Edit the first line of `recipe.txt` to include `chop 2 avocados`
  * Commit and Push the changes to Github.
* Collaborator:
  * Edit the first line of `recipe.txt` but swap `avocados` for `mangos`
  * Try pushing to GitHub.
* To start solving the _conflict_ let's pull the latest version of the code:
  * `git pull origin master`: leads to a CONFLICT
  * pull = fetch + merge. Merge step failed.
  * `cat recipe.txt`: indicates differences between the versions.
    * `====` indicates separation between versions
    * `<<<<< HEAD` precedes OUR change
    * `>>>>>> <hash>` below remote change
  * Restore the file to the original version manually:
    * `nano recipe.txt` 
    * Delete conflict.
    * `git add`
    * `git status`
    * `git commit`
    * `git push`
* Collaborator:
  * Pull newest version: `git pull origin master`
  * No merge conflict. Git knows how to replay changes!
* Tips to avoid conflicts:
  * Make smaller commits. Easier to manage. Harder to conflict.
  * Always pull from upstream before starting to work. Pull often as you work.
  * Communicate.

## Licensing and Open Science
* Sharing is caring.
* Code (and data) on Github is usually under open-source licenses that permit re-use while maintaining authorship.
* Some open-source licenses are more 'open' than others:
  * The GPL family forces derivative works to abide by its terms (copyleft).
  * The MIT and Apache Licenses (among others) do not have this requirement.
* Creating releases on Github is useful to mark important milestones of your project.
* You can associate your repository with Zenodo to have DOIs issued for every release: citations!

## Gists
* A Gist is essentially a single-file repository.
* Useful to share small scripts and notes (like this document).
* Public (all can search/read) or Private (only those with the URL can read).



---

This material is licensed under CC-BY 4.0 2019-2020 by João Rodrigues.