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
