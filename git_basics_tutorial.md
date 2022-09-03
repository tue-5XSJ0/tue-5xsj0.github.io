# Git Basics
Git is a powerful but lightweight version control system with a lot of features. As all the commands, and especially the
specific direction of these commands can be daunting at first, this guide (and cheatsheet) tries to shed some light on
it. It is recommended to first try this on your own private Github repository to get a feel of what these commands do.
We have tried to build it up from the basics and keep it elemental, for more advanced users this might all be familiar 
already. In either case, [the documentation](https://git-scm.com/docs/) is a good place to look at when you want to know 
the details/accepted flags of certain commands.


## Set-up
First, it is needed to install Git for your OS. In Windows, this can be done through 
[their website](https://git-scm.com/download/win), whereas in Debian/Ubuntu Linux `apt-get install git`
suffices (see [here](https://git-scm.com/download/linux) for other distributions).

The first thing you want to do is set-up your user information to be used when reviewing version history:
```
git config --global user.name "firstname lastname"
git config --global user.email "valid-email"
```

What you can do with Git now is creating a folder where git will keep track of its contents, this is called a 
repository. These contents can either be made (from scratch) by you, for this you want an empty folder to start with 
using:

``` git init ``` 

It is also possible to grab code from an existing repository and keep track of that (remotely) in your folder. For this
you would use:
 
``` git clone ssh://user@domain.com/repo.git ```

which uses SSH to fetch the repository with code 
(and hence needs a pair of public/private keys to communicate with the server which you need to generate and add 
yourself). Git also allows to sync with a repository over HTTPS, for this you would instead use 
`git clone https://domain.com/repo.git`, where your password can be cached using a 
[credential helper](https://help.github.com/en/github/using-git/caching-your-github-password-in-git). 
A more extensive overview of allowed protocols and options can be found 
[here](https://www.git-scm.com/docs/git-clone#_git_urls_a_id_urls_a).

## Changes
Now that you have a working directory where your repository is being tracked (either with existing code, or your own), 
you can start to make changes to the files in that folder (using your favorite editor). These changes will then be 
automatically recorded by Git. Using the commandline, navigate to the folder you want to check out and have already 
initialised Git in (you can 'add' Git to as many folders as you like). Now you can check the status of your local 
changes with respect to the 'origin' (i.e. the code on the server, where you as a team work on). This is done with:

``` git status ```

When you are happy with your changes, and want to get these onto the server, you need to 'commit' your changes. Git 
provides a way to document these 'commits' so that when something breaks, you can (more) easily track back to when the 
code did work. First, you need to 'add' your local changes to a specific commit. This is done with:

``` git add <file> ```

Although it is not recommended to add a lot of files to one commit, as you want the commits to be as 'atomic' (small) as 
possible making it easier to see how the code is changed with that commit, it is possible to add all changed files to 
a commit using:

``` git add . ```

Now, it is possible to finalise your commit with a useful message. The message should be simple and descriptive of the
 changes made, but still be short as not to clutter the overview of commits. This can be done with:

``` git commit -m"Add useful description of changes (roughly <50 characters)" ```

If you - in the meantime - needed to add some code which should be included in that commit, you can 'add' the files 
again and use the following command to change your last commit (NB: Don't ammend published commits!):

``` git commit --ammend ```

Finally, you can get these changes onto the remote server with:

``` git push ```


[comment]: <> (## Branches and organization)

[comment]: <> (All the previous commands would work on the default place in Git, the 'master' branch. In Git, a branch can be seen as )

[comment]: <> (a 'train track' branching off from a code base where development will not interfere with the code on the main 'master' )

[comment]: <> (branch. This way, you can develop features parallel to working code on the 'master', and later merge these into 'master')

[comment]: <> (once you have tested them to work. Therefor, it is not recommended to push your changes straight to this master branch.)

[comment]: <> (For this, you can create branches. Again, it is important to make these names descriptive. A rule of thumb is to prepend)

[comment]: <> (the branches with '/feature/', '/fix/', or '/test/'. This makes it easier to make sense of &#40;or filter&#41; the branches when )

[comment]: <> (you have a lot of them. Furthermore, although using `git blame <file>` reveals who changed what, it is )

[comment]: <> (recommended to append the branch name with your initials. An example from John Doe fixing the communication with an app)

[comment]: <> (would then be:)

[comment]: <> (``` /fix/app_communication_jd ```)

[comment]: <> (To create branches, it is always good to make sure your current workspace is up to date with the code on the server.)

[comment]: <> (This can be done with:)

[comment]: <> (``` git pull ```)

[comment]: <> (Then, it is possible to create a branch &#40;assuming you are still on the 'master' branch, else switch to that branch and)

[comment]: <> (pull again using the command explained later&#41;. This can be done with:)

[comment]: <> (``` git checkout -b </prepend/branch_name_initials> ```)

[comment]: <> (This sets up a branch both locally and on the remote &#40;i.e. server&#41; and switches to it. If you want to switch to an )

[comment]: <> (existing branch, drop the `-b` flag.)


[comment]: <> (## Remotes)

[comment]: <> (The above assumes you have set up only one remote with which git communicates &#40;e.g. only your own Github account&#41;, )

[comment]: <> (however, Git makes it possible to have multiple remote locations and can switch between them. This requires some )

[comment]: <> (extra arguments in the previous commands, and introduces some new.)

[comment]: <> (Firstly, you can list all the added remotes with:)

[comment]: <> (``` git remote -v ```)

[comment]: <> (And show information about a specific remote with:)

[comment]: <> (``` git remote show <remote> ```)

[comment]: <> (It is now possible to add a new remote repository using the following. This is also done 'behind-the-scenes' when you )

[comment]: <> (for example use `git clone`.)

[comment]: <> (``` git remote add <shortname> <url> ```)

[comment]: <> (Now you can download all changes from the remote, but not yet integrate them into your local files specified with HEAD)

[comment]: <> (&#40;more on that later&#41;.)

[comment]: <> (``` git fetch <remote> ```)

[comment]: <> (If you do want them integrated into your local Git HEAD, you can use the following:)

[comment]: <> (``` git pull <remote> <branch> ```)

[comment]: <> (Conversely, you can also push your local commit&#40;s&#41; to a specific remote + branch:)

[comment]: <> (``` git push <remote> <branch> ```)


[comment]: <> (## Merging and HEAD)

[comment]: <> (The second to last command &#40;`git fetch`&#41; only grabs the changes from the server, but does not include them )

[comment]: <> (yet. For this, you need to merge these changes into your current HEAD. This HEAD is basically a file which tracks all)

[comment]: <> (the changes you have made locally. )

[comment]: <> (``` git merge <branch> ```)

[comment]: <> (Graphically, it looks like this before merging:)

[comment]: <> (```)

[comment]: <> (	  A---B---C branch)

[comment]: <> (	 /)

[comment]: <> (    D---E---F---G current)

[comment]: <> (```)

[comment]: <> (After merging:)

[comment]: <> (```)

[comment]: <> (	  A---B---C branch)

[comment]: <> (	 /         \)

[comment]: <> (    D---E---F---G---H current)

[comment]: <> (```)

[comment]: <> (If, in a more complicated situation, the remote 'master' is ahead of your current branch, you can perform a rebase to)

[comment]: <> (get the commits from master, and then re-apply the changes of your branch. This helps if there is a bug for example, )

[comment]: <> (which is fixed in the 'master' branch after you've created your branch. If you need to have this fix as well, you need)

[comment]: <> (to perform a rebase, which graphically looks like this:)

[comment]: <> (Before `git rebase master`:)

[comment]: <> (```)

[comment]: <> (          A---B---C topic)

[comment]: <> (         /)

[comment]: <> (    D---E---F---G master)

[comment]: <> (```)

[comment]: <> (After `git rebase master`:)

[comment]: <> (```)

[comment]: <> (                  A'--B'--C' topic)

[comment]: <> (                 /)

[comment]: <> (    D---E---F---G master)

[comment]: <> (```)

[comment]: <> (Inevitably, there will be conflicts between the files you have changed and the files changed on the remote &#40;lucky you)

[comment]: <> (if there aren't any!&#41;. These can be resolved manually by adding/removing edited conflicted files using:)

[comment]: <> (``` git add <resolved-file> ```)

[comment]: <> (``` git rm <resolved-file> ```)

[comment]: <> (However, it is oftentimes easier to use a graphical tool to solve the conflicts. You can configure any you like &#40;or any )

[comment]: <> (built into your IDE&#41; using `git config merge.tool <mergetool>`. Common graphical ones are `kdiff3` or )

[comment]: <> (`meld`. Now, you can resolve conflicts using:)

[comment]: <> (``` git mergetool ```)

[comment]: <> (## Undo)

[comment]: <> (A great benefit of using Git is that you can undo/roll-back changes you have &#40;erroneously or not&#41; made. Be really )

[comment]: <> (careful with the use of the commands below though, because using the flag `--hard` cannot be undone!)

[comment]: <> (When you want to discard all local changes in your directory, use:)

[comment]: <> (``` git reset --hard HEAD ```)

[comment]: <> (Which can also be done for a specific file using:)

[comment]: <> (``` git checkout HEAD <file> ```)


[comment]: <> (Now, if you want to go back to a certain commit, there are two ways. One is to create a new commit undoing all the )

[comment]: <> (previous commits. This is done using:)

[comment]: <> (``` git revert <commit> ```)

[comment]: <> (Otherwise, when you want to reset your HEAD to a previous commit and throw away all your commits, use:)

[comment]: <> (``` git reset --hard <commit> ```)

[comment]: <> (Resetting your HEAD, but preserve all changes in those commits as unstaged changes:)

[comment]: <> (``` git reset <commit> ```)

[comment]: <> (Or finally, resetting your HEAD, preserve all changes in those commits as unstaged changes and preserve local )

[comment]: <> (uncommitted changes:)

[comment]: <> (``` git reset --keep <commit> ```)

[comment]: <> (## Issues and Pull Requests)

[comment]: <> (One of the great things of &#40;open source&#41; software development is the possibility to let others &#40;the maintainers&#41; know )

[comment]: <> (that either something does not work or can be improved. These can be conveyed using **issues** &#40;letting them know&#41; and )

[comment]: <> (**pull requests** &#40;fixing/improving it&#41;. )

[comment]: <> (#### Issues)

[comment]: <> (Issues can be opened from the repository home page on GitHub &#40;example for this course is the [tutorials )

[comment]: <> (repo]&#40;https://github.com/tue-5AUA0/tue-5aua0.github.io]&#41;&#41;. It is important to give it a good but short descriptive)

[comment]: <> (title &#40;preferably fewer than ~60 characters&#41;, which is true in general for titles for which more information can be )

[comment]: <> (found [here]&#40;https://www.nngroup.com/articles/microcontent-how-to-write-headlines-page-titles-and-subject-lines/&#41;.)

[comment]: <> (Also, assign a proper label for the issue making it easier to filter. In the body of the issue, try to be as thorough as)

[comment]: <> (possible. If a specific error/bug is involved, summarize the system you are working on &#40;software version&#40;s&#41;, package )

[comment]: <> (version&#40;s&#41;, your OS + version&#41;. )

[comment]: <> (Furthermore, make use of the supported markdown to make it readable &#40;headers, lists, italics, bold, etc.&#41;.)

[comment]: <> (Please also link relevant code snippets in here &#40;for which a tutorial can be found )

[comment]: <> ([here]&#40;https://help.github.com/en/github/managing-your-work-on-github/creating-a-permanent-link-to-a-code-snippet&#41;&#41;.)

[comment]: <> (#### Pull requests)

[comment]: <> (When there is a bug-fix or improvement you want to see implemented in the code, you can make a pull request &#40;which is )

[comment]: <> (similar to a merge request if you work on a private code base in GitLab&#41;. This links the branch you made your )

[comment]: <> (improvement on and a target &#40;most often 'development' or 'master'&#41;. In here, you can indicate why you want to make this)

[comment]: <> (change and/or addition and what you have done. It is also possible to assign someone else &#40;e.g. a maintainer&#41; to have a)

[comment]: <> (look at it and possibly merge it. After you have cloned the repository, you can create this branch containing your )

[comment]: <> (changes. )

[comment]: <> (Now, on the GitHub repository page, you can create a pull request linking this branch and write a description/summary )

[comment]: <> (for it. When there is an issue which is solved by this pull request, it is also possible to link these &#40;which of course)

[comment]: <> (then should be done!&#41;. )

[comment]: <> (As a reference to what constitutes a good pull request, please also see [this GitHub )

[comment]: <> (tutorial]&#40;https://gist.github.com/mikepea/863f63d6e37281e329f8&#41;.)