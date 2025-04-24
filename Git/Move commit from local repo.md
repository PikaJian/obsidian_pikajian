# Move commit from local repo

Yes, by adding a remote and cherry-picking the commits on top of your first commit.
```
# add the old repo as a remote repository
git remote add oldrepo https://github.com/path/to/oldrepo

# get the old repo commits
git remote update

# examine the whole tree
git log --all --oneline --graph --decorate

# copy (cherry-pick) the commits from the old repo into your new local one
git cherry-pick sha-of-commit-one
git cherry-pick sha-of-commit-two
git cherry-pick sha-of-commit-three

# check your local repo is correct
git log

# send your new tree (repo state) to github
git push origin master

# remove the now-unneeded reference to oldrepo
git remote remove oldrepo


```

* * *

The rest of this answer is if you still want to add the LICENSE to your previous repo.
Yes. You can place your LICENSE commit as the first commit by rebasing.
Rebasing is gits way of rearranging commit order while keeping all the commit authors and commit dates intact.
When working on a shared repo, it's generally discouraged unless your entire team is git-fluent. For those that aren't, they can just clone a fresh copy of the repository.
Here's how you get your LICENSE commit as the first commit.

# 1\. Update and rebase your local copy

Check out your project and place the LICENSE file in a commit ON TOP of your current 3 commit stack.
```
#create LICENSE file, edit, add content, save
git add LICENSE
git commit -m 'Initial commit'


```
Then do an interactive rebase on the master branch to **REARRANGE** the commits.
```
git rebase -i --root


```
It will open an editor. Move the bottom line (your "Initial commit" commit, the most recent commit) to the top of the file. Then save and quit the editor.
As soon as you exit the editor, git will write the commits in the order you just specified.
You now have your local copy of the repository updated. do:
```
git log


```
to verify.

# 2\. Force push your new repo state to github

Now that your copy is updated, you have to force push it to github.
```
git push -f origin master


```
This will tell github to move the master branch to its new location. You should only force push in rare occasions like this where everybody working with it is aware of the pending change, else it will confuse your collaborators.

# 3\. Synchronize collaborators to github

Lastly, all the collaborators will have to synchronize to this repository.
First **they must have clean repositories** as the following command can be destructive if there are unsaved changes.
```
# make sure there are no unsaved changes
git status

# pull the latest version from github
git fetch 

# move their master branch pointer to the one you published to github.
git reset --hard origin/master


```
That's it. Everybody should be in sync now.
