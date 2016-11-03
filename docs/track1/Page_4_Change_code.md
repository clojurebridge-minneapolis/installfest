# Chapter 4: Changes and Commits

In this section, we will alter the code. We will start with something small and fix the `README.md.`

#### Changing README.md

Open the `README.md` file in your editor and replace the two "FIXMEs" with different text. Save the file.

Now, if you ask git for the status, it will show that `README.md` has changed.

    $: git status
    On branch master
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
    On branch master
    Changes not staged for commit:
      (use "git add <file>..." to update what will be committed)
      (use "git checkout -- <file>..." to discard changes in working directory)

        modified:   README.md
        modified:   project.clj

    no changes added to commit (use "git add" and/or "git commit -a")

#### Adding And Committing The Changes

Now that our the changes in the editor are saved, let's add and commit the changes. _Adding_ files prepares to upload the updated files. _Committing_ actually records a snapshot of files -- it puts a timestamp and version description of the current state of the project.

First, _add_ the specific file(s) with "`git add` + file name":

    $: git add README.md project.clj

Confirm the file was added by checking the status:

    $: git status
    On branch master
    Changes to be committed:
      (use "git reset HEAD <file>..." to unstage)

	modified:   README.md
	modified:   project.clj

Two files have been added (modified) and the changes can be
_committed_. When you enter the `git commit` command, make sure to enter a description of the changes you are committing; this will help you identify versions and changes. Add a quick note after the file names `-m "comment on changes"`.

    $ git commit -m "fixing README.md description"
    [master 57aff88] fixing README.md description
     2 files changed, 3 insertions(+), 3 deletions(-)


Check the status again:

    $: git status
    On branch master
    nothing to commit, working directory clean

Git status reports no uncommitted changes because we successfully committed the updated files.

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

You see two commits: the last commit from our initial commit and our new commit (fixing README.md description).

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
