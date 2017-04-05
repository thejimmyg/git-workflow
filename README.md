# Git Notes

This is for working on a master branch on a fork.

1. On GitHub, fork the repo, using the button at the top right.

   ```
   https://github.com/upstream/repo.git ->  https://github.com/fork/repo.git
   ```

   The fork you've made will be your *origin* remote.

2. Get your fork locally:

   ```
   git clone https://github.com/fork/repo.git ~/path/repo
   cd ~/path/repo
   ```

   You'll also need access to the original repo for fetching changes. This is
   known as the *upstream* remote:

   ```
   git remote add upstream https://github.com/upstream/repo.git
   ```

   To prevent problems, add a `pushurl` line to the end of the `.git/config` file

   ```
   [remote "upstream"]
       url = https://github.com/upstream/repo.git
       fetch = +refs/heads/*:refs/remotes/upstream/*
       pushurl = no_push
   ```

3. Do some work

   ```
   git add file1 file2
   git commit -m "My changes"
   # Push to your origin fork on GitHub
   git push origin master
   ```

4. Rebase your changes on top of latest master

   ```
   git fetch upstream
   git rebase upstream/master
   git status
   # Resolve the problem
   # e.g. vim package.json
   git add package.json
   git merge --continue
   git status
   # Check it is OK
   git log
   ```

   Since this takes a while, repeat the above again in case there are more changes.

5. Publish your rebased branch

   Then when you are sure everything is up to date:

   ```
   git push origin master
   ```

6. Create pull request on github

   Make any extra changes requested, and push them to the branch on origin that
   contains your pull request for that pull request to be updated.

7. The upstream maintainer will merge your pull request using the Merge option (not rebase or squash).

8. Now you can pull in the upstream changes of your changes:

   ```
   git fetch upstream
   git merge upstream/master
   # Deal with any merge conflicts using `git add`, then `git commit`
   git status
   ```

   4. If everything goes well, there won't be any changes because your origin and
   local copy will both be in exactly the same state as upstream.

9. Every time you start working, repeat 8.

   This is so you get the latest changes before you start, which will reduce
   the chance of any conflicts at the end.

PRO tip: Use tab completion on everything to avoid mistakes, use `git status` a
lot, it gives you hints as to the commands you might want to use next.
