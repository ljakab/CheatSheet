Git Cheat Sheet
===============

Configure global parameters:

    git config --global user.name "<name>"
    git config --global user.email "<email>"
    git config --global color.ui "auto"
    git config --global push.default "matching"
    git config --global alias.st "status"
    git config --global alias.ci "commit"
    git config --global alias.co "checkout"
    git config --global merge.tool "kdiff3"

List global parameters:

    git config --global --list

Import existing project into a git repo managed by `gitolite`: in the directory gitolite-admin edit `gitosis.conf` accordingly, and then

    git commit -a -m "Added project ..."
    git push

In the project directory

    cd project
    git init
    git add .
    git commit -m "Initial import of project"
    git remote add origin <URL>
    git push origin master

Forget about local changes

    git checkout -f master
    git checkout -- <filename>

Set up tracking after checkout

    git branch --set-upstream master origin/master

Remove last commit

    git reset --hard HEAD^

Delete local branch

    git branch -d <branch>

Delete remote branch

    git push origin :<branch>

Untrack a file

    git rm --cached <filename>

Undo changes (not a good idea with shared repos)

    git reset --hard <commit>

... and if there is work to keep

    git stash
    git reset --hard <commit>
    git stash pop

Branching
---------

Create new branch

    git checkout -b newbranch

Push branch to remote repository

    git push origin newbranch

Get remote branches

    git fetch origin
    git checkout -b anotherbranch origin/anotherbranch
or

    git checkout --track origin/anotherbranch

Delete a remote branch/tag

    git push origin :remotebranch
    git push origin :refs/tags/remotetag

Other
-----

Check if a patch applies well

    git apply --check --whitespace=error-all patch

Clean trailing whitespace

    git diff | git apply --whitespace=fix -

Push to all remotes with one command

    git remote add all origin-host:path/proj.git
    git remote set-url --add all another-host:path/proj.git
    git remote set-url --add all third-host:path/proj.git

    git push all --all

Change date of previous commits

    git rebase -i master
    git commit --amend --date=`date`
    git rebase --continue

Show TABs in indent as errors with git diff --check

    git -c core.whitespace=tab-in-indent diff master --check

Merge branch, with "theirs" strategy

    git checkout target
    git merge -X theirs source

Configure git-review to use https instead of ssh

    git config --global gitreview.scheme https
    git config --global gitreview.port 443
