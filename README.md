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
