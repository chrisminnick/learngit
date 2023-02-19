- Git is a Distributed Revision Control System
- Git is a Revision Control System
- Git is a stupid content tracker - its a versioned file system
- Git is a persistant map - it stores key/value pairs

## Git stores files as key - value pairs

- The key is a 20-character SHA1 hash of the file content.
- You can generate the has for any content using git hash-object
  echo "testing" | git hash-object --stdin

- hash-object is a plumbing command

- The commands you generally use are "porcelin" commands

- To save the result of hash-object in the repo, use the -w option

First you need to initialize a repo:
git init

Then hash and save:

`echo "testing" | git hash-object --stdin -w`

Open the .git folder: `open .git`

Look in the objects folder.
The folder names are the first two characters of the hash, and the files in the folder are the other 18 characters. The contents of the files are compressed and contain header information, but you can read those with cat-file:

`git cat-file 777c1b747cb60696dee3f5c840246fb0a244a7fa -p`

You can also just use the first few characters of the hash.

Git has four different kinds of objects:

- blobs - data
- trees - contain blobs and other trees
- commits - keep versioning information
- annotated tags

The names of blobs aren't stored in the blob, they're stored in the trees. So, the same object can be pointed to by different trees, with different names.

Git is a high-level file system built on top of your native file system.

## Git Branches

Git creates a branch when you do your first commit.

view branches with git branch

What is a branch?
Branches are stored in refs/heads in .git

The contents of the files in refs/head aren't compressed. A branch file contains the hash of the current commit.

A branch is just a reference to a commit.

The main branch doesn't have any special status in Git.

Main is a better name for the first branch than master, because it's not really the master of anything. It's just like any other branch.

You can actually create a new branch by writing a file to refs/heads containing the hash of a commit.

To create a new branch, use `git branch branchname`

The current branch is stored in the HEAD
`cat .git/HEAD`

HEAD is a reference to a branch.

To switch to a different branch:
`git switch nameofbranch`
You can also use git checkout, and it works the same in this case. Switch is a more recent command.

When you switch to a different branch, Git changes HEAD to point to that branch and replaces the files and folders in the working directory to the ones in the commit pointed at by the branch.

## Merging branches

Switch back to the main branch and merge in changes with git merge branchname

If their are conflicts, you'll need to manually resolve them, then stage the merged file(s) then commit.

git add filename
git commit

When you do the merge, git will give you a suitable commit message.

### A merge is a commit with two parents.

References in commits are used to track history.

References in trees are used to track content. Retrieving a past state in git just goes into a commit and retrieves the trees and blobs from there. That's it!

Git doesn't care about your working directory. When you move to another commit, Git just tracks trees and branches and loadings them according to the commit that's the HEAD. Objects are immutable. The working directory changes when you use git switch.

When you merge from an ancestor, Git could do it the same way as when you merge a branch back into main, but that would be wasteful. Instead, git will make the branch just point to the latest commit of the branch you're merging.

For example, if you create a branch named ideas from main, then make changes to main but not to ideas, you can make ideas point to the latest same commit as main by doing git merge main while ideas is the working directory. This is called a fast-forward.

You can checkout any commit using git checkout. The head will move to that commit. This is called a detached head because you're not on a branch. If you do commits in a detached head and then switch back to a branch, you leave those commits behind and they're unreachable unless you remember the hashes. Git will clean those up later (garbage collection). To save those, you can check them out and put a branch on it.

## Rebasing

Merging creates a commit with two parents. If you instead want to change the base of a branch so that it will contain all the commits from the original branch plus the commits made to the branch, you can use rebase.

`git rebase main`

Git rebase rearranges branches so they look like a single branch. What it actually does is to make copies of the branch's commits and adds them to the branch you're rebasing to, then moves the branch to the new commits. The old commits become impossible to reach and will be garbage collected.

Why do we need rebase?
Merging preserves history exactly as it happened.

Understanding Tags
A tag is a label for a commit.

git tag tagname
or
git tag tagname -a -m 'First release'

git tag
//lists the tags

You can checkout tags by using git checkout.
git checkout tagname

Tags are kept in refs/tags

cat .git/refs/tagname
shows that the content is a hash

git cat-file -t b93f3345
Shows that the type of the hash is tag

git cat-file -p b93f3345
Shows that the content of the tag is a commit and an annotation.

A tag is a reference (like a branch) to a commit.
If you make a tag without -a, it will point directly to the commit.
