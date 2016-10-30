# Chapter 4: Branches, Merges, Commits

In this section, we will alter the code. We will start with something small and fix the `readme.md.` Before we do that, we want to branch the code to ensure we can track the changes we make. Branching the code is a version control method; we keep the original files on the master branch and create a separate branch where we will make the changes. When we are confident our changes are correct, we will merge the new branch to the existing master branch. The updated files on the new branch essentially overwrite the files on the master branch.

#### <a name="branch-the-code">Branch the code</a>

Branching tells git that we are making changes to the current set of files and that we want to have a specific name for this version. To check what branch you are currently on, enter:

    $: git status
    On branch master
    Your branch is up-to-date with 'origin/master'.
    nothing to commit, working directory clean

This tells us we are currently on the master branch. This is where the original code lives, we want to create a new branch where we can make changes. To create a new branch, use the "git branch" command and name the new branch. Let's call our new branch "fix-readme". This is a reminder to ourselves what kind of changes we are making in this new branch.

    $: git branch fix-readme

If you enter `git branch` without the specific branch name, the terminal will list all branches that exist in this directory. 

    $: git branch
    fix-readme
    * master

Notice there are now two branches, `master` and `fix-readme`. The asterisk (*) indicates which branch you are on. In the command above, you see we are on the master branch. We want to switch to the `fix-readme` branch. We do this by "checking out" the branch we want to be on. Then we will confirm we are on the new branch.

    $: git checkout fix-readme
    Switched to branch 'fix-readme'


    $: git branch
    * fix-readme
    master


    $: git status
    On branch fix-readme
    nothing to commit, working directory clean

We've confirmed we are now on the `fix-readme` branch.

It's important to check out the branch when making changes. Git does not do it automatically, so you can end up committing changes to the `master` branch. This usually isn't disastrous, but it's often very messy to clean up if things go wrong and it's difficult to keep track of versions.

#### Changing README.md

Open the `README.md` file in your editor and replace the two "FIXMEs" with different text. Save the file.

Now, if you ask git for the status, it will show that `README.md` has changed.

    $: git status
    On branch fix-readme
    Changes not staged for commit:
    (use "git add <file>..." to update what will be committed)
    (use "git checkout -- <file>..." to discard changes in working directory)

        modified:   README.md

    no changes added to commit (use "git add" and/or "git commit -a")

We can ask git to show us exactly what changed.


    $: git diff
    diff --git a/README.md b/README.md
    index 9493433..718893f 100644
    --- a/README.md
    +++ b/README.md
    @@ -1,6 +1,6 @@
    # chatter

    -FIXME
    +This is a web app that will display posted messages.

     ## Prerequisites

    @@ -16,4 +16,4 @@ To start a web server for the application, run:

     ## License

    -Copyright © 2014 FIXME
    +Copyright © 2014 clojurebridgemn.org


`git diff` is telling us the `README.md` file changed. In the example above, we removed the line that said "FIXME" and added a line saying "This is a web app that will display posted messages." We also changed the FIXME in the copyright to an email address. You should see the specific changes you made in your command line.

Since we changed the description in the `README.md`, we might as well change the description in `project.clj` file. Make the change and save that file too. Now the git status should be:


    $: git status
    On branch fix-readme
    Changes not staged for commit:
      (use "git add <file>..." to update what will be committed)
      (use "git checkout -- <file>..." to discard changes in working directory)

        modified:   README.md
        modified:   project.clj

    no changes added to commit (use "git add" and/or "git commit -a")

#### Adding And Committing The Changes

Now that our the changes in the editor are saved, let's add and commit the changes to our new branch. _Adding_ files to the branch puts the updated files on the branch. _Committing_ the updated files to the branch saves creates a specific version of the branch - it puts a timestamp and version description of the current state of the branch. You must add, then commit files to the branch.

First, _add_ the specific file(s) with "`git add` + file name" to the branch you are on:

    $: git add README.md project.clj

Confirm the file was added by checking the status:

    $: git status
    On branch fix-readme
    Changes to be committed:
      (use "git reset HEAD <file>..." to unstage)

	modified:   README.md
	modified:   project.clj

Two files have been added (modified) and the changes can be _committed_ to the branch. When you enter the `git commit` command, make sure to enter a description of the changes you are committing; this will help you identify versions and changes. Add a quick note after the file names `-m "comment on changes"`.

    $ git commit -m "fixing README.md description"
    [fix-readme 57aff88] fixing README.md description
     2 files changed, 3 insertions(+), 3 deletions(-)


Check the branch status again: 
    
    $: git status
    On branch fix-readme
    nothing to commit, working directory clean

Git status reports no uncommitted changes because we successfully committed the updated files to the `fix-readme` branch. 

You can also the check the log to see what commits have been made, when they were made, and the brief comment on the commit.

    $: git log
    commit 57aff889c81698394faf8568b63f14130d32599a
    Author: crkoehnen <crkoehnen@gmail.com>
    Date:   Sun Dec 28 17:33:41 2014 -0600

        fixing README.md description

    commit 44a560f1653770afac01aea2c9279a7af46a46eb
    Author: crkoehnen <crkoehnen@gmail.com>
    Date:   Sun Dec 28 16:43:37 2014 -0600

        initial commit

You see two commits: the last commit from our original branch (initial commit) and our new commit (fixing README.md description).

#### Merging The Changes

Once we are satisfied with the updated files and they are successfully committed on the `fix-readme` branch, we'll merge those changes on to the `master` branch. This means we are going to overwrite the existing files (or add new files) on the master branch.

First, go to the `master` branch and check its log. 

    $: git checkout master
    Switched to branch 'master'
    Your branch is up-to-date with 'origin/master'.

    $: git log
    commit 44a560f1653770afac01aea2c9279a7af46a46eb
    Author: crkoehnen <crkoehnen@gmail.com>
    Date:   Sun Dec 28 16:43:37 2014 -0600

        initial commit

Note that `master` is lagging behind the `fix-readme` branch. It doesn't have the commit with the README changes. We will fix that by merging our `fix-me` branch into the `master` branch.

    $: git merge fix-readme
    Updating 44a560f..57aff88
    Fast-forward
     README.md   | 4 ++--
     project.clj | 2 +-
     2 files changed, 3 insertions(+), 3 deletions(-)

The merge brought in the changes. If we check the log, we'll see two commits now. The initial commit and our "readme.md" changes.

    $: git log
    commit 57aff889c81698394faf8568b63f14130d32599a
    Author: crkoehnen <crkoehnen@gmail.com>
    Date:   Sun Dec 28 17:33:41 2014 -0600

        fixing README.md description

    commit 44a560f1653770afac01aea2c9279a7af46a46eb
    Author: crkoehnen <crkoehnen@gmail.com>
    Date:   Sun Dec 28 16:43:37 2014 -0600

        initial commit

#### Deleting the fix-readme branch

Now that we've pulled the changes from the `fix-readme` branch into `master`, we no longer need the `fix-readme` branch, so let's delete it.

    $: git branch -d fix-readme

#### Pushing to GitHub

The final step will be to push our changes to GitHub. This allows other people to view our code changes. Enter the following (you will use your own username and password when prompted):

    $: git push origin master
    Username for 'https://github.com': crkoehnen
    Password for 'https://crkoehnen@github.com':
    Counting objects: 4, done.
    Delta compression using up to 4 threads.
    Compressing objects: 100% (4/4), done.
    Writing objects: 100% (4/4), 572 bytes | 0 bytes/s, done.
    Total 4 (delta 2), reused 0 (delta 0)
    To https://github.com/crkoehnen/chatter.git
       44a560f..57aff88  master -> master

Refresh GitHub in your browser and you will see the text and the commit count change in your repository page. Your changes are now available for collaborators to review!


That's all the command line git we will need for this tutorial. GitHub maintains a list of resources where you can learn more about git and GitHub.

In [Chapter 5](Page_5_More_code_changes.md), we'll make more to the code.

[Good Resources for Learning Git and GitHub](https://help.github.com/articles/good-resources-for-learning-git-and-github).
