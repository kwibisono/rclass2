---
title: "Introduction to Git and GitHub II"
author: 
date: 
urlcolor: blue
output: 
  html_document:
    toc: true
    toc_depth: 3
    toc_float: true # toc_float option to float the table of contents to the left of the main document content. floating table of contents will always be visible even when the document is scrolled
      #collapsed: false # collapsed (defaults to TRUE) controls whether the TOC appears with only the top-level (e.g., H2) headers. If collapsed initially, the TOC is automatically expanded inline when necessary
      #smooth_scroll: true # smooth_scroll (defaults to TRUE) controls whether page scrolls are animated when TOC items are navigated to via mouse clicks
    number_sections: true
    fig_caption: true # ? this option doesn't seem to be working for figure inserted below outside of r code chunk    
    highlight: tango # Supported styles include "default", "tango", "pygments", "kate", "monochrome", "espresso", "zenburn", and "haddock" (specify null to prevent syntax    
    theme: default # theme specifies the Bootstrap theme to use for the page. Valid themes include default, cerulean, journal, flatly, readable, spacelab, united, cosmo, lumen, paper, sandstone, simplex, and yeti.
    df_print: tibble #options: default, tibble, paged
    keep_md: true # may be helpful for storing on github
---

# Git commands: Observing your repository

Below are some common git commands you might use to observe your repository:

- [git status](#git-status)
- [git log](#git-log)
- [git diff](#git-diff)

## `git status`

**`git status`**: Shows the working tree status

- Help: `git status -help`
- Syntax: `git status [<option(s)>]`
    - Commonly used without any options, but see help file for possible options
- Output:
    - Information about the branch (e.g., which branch you are on, its status relative to the remote branch)
    - `Changes to be committed`
      - List of files that have been added to the _staging area_ using `git add`
      - These can be committed using `git commit`
      - The filenames will be in <span style="color: green;">green</span>
    - `Changes not staged for commit`
      - List of tracked files (i.e., files that have been added using `git add` before) that have since been changed (e.g., modified, deleted) in the _working directory_
      - These can be added to the _staging area_ using `git add`
      - The filenames will be in <span style="color: red;">red</span>
    - `Untracked files`
      - List of untracked files (i.e., new files that have never been added using `git add` before)
      - These can be added to the _staging area_ using `git add`
      - The filenames will be in <span style="color: red;">red</span> 

Below is a sample output of `git status`:

```
On branch master
Your branch is up-to-date with 'origin/master'.

Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

	new file:   clean_dataset.R

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   create_dataset.R

Untracked files:
  (use "git add <file>..." to include in what will be committed)

	analyze_dataset.R
```

<br>
<details><summary>**Example**: Checking `git status` after creating a new file</summary>

- Imagine you have created a new file called `create_dataset.R` in your git repository
- You will initially see the file listed under `Untracked files`


```bash
# Create new R script
echo "library(tidyverse)" > create_dataset.R
echo "mpg %>% head(5)" >> create_dataset.R

git status
```

```
On branch master
Your branch is up-to-date with 'origin/master'.

Untracked files:
  (use "git add <file>..." to include in what will be committed)

	create_dataset.R

nothing added to commit but untracked files present (use "git add" to track)
```
</details>

<br>
<details><summary>**Example**: Checking `git status` after adding a file</summary>

- After adding `create_dataset.R`, you will see it listed under `Changes to be committed`


```bash
# Add R script
git add create_dataset.R

git status
```

```
On branch master
Your branch is up-to-date with 'origin/master'.

Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

	new file:   create_dataset.R

```
</details>

<br>
<details><summary>**Example**: Checking `git status` after making a commit</summary>

- After making a commit, you will notice that the committed file(s) are no longer listed
- If your local repository is connected with a remote, you'll also see that it says your branch is ahead of the remote by 1 commit


```bash
# Make a commit
git commit -m "add create_dataset.R"

git status
```

```
On branch master
Your branch is ahead of 'origin/master' by 1 commit.
  (use "git push" to publish your local commits)

nothing to commit, working tree clean
```
</details>

<br>
<details><summary>**Example**: Checking `git status` after modifying a tracked file</summary>

- If you make further modifications to a file that's being tracked (i.e., a file that's been added before), you will see it listed under `Changes not staged for commit` (as compared to under `Untracked files` when it's never been tracked before)


```bash
# Modify create_dataset.R
echo "df <- mpg %>% filter(year == 2008)" >> create_dataset.R

git status
```

```
On branch master
Your branch is ahead of 'origin/master' by 1 commit.
  (use "git push" to publish your local commits)

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   create_dataset.R

no changes added to commit (use "git add" and/or "git commit -a")
```
</details>
<br>

## `git log`

**`git log`**: Show commit logs

- Help: `git log -help`
- Syntax: `git log [<option(s)>]`
  - Commonly used without any options, but see help file for possible options
- Output: List of commits in reverse chronological order (i.e., newest first)
  - `commit <commit_hash>`: Each commit can be uniquely identified by their hash ID (SHA-1)
    - _**Note**: Only the first 7 characters of this hash is needed to uniquely identify it_
  - `Author: <username> <email>`: Username and email of the author of the commit
  - `Date: <commit_date>`: Date of the commit
  - `<commit_message>`: Commit message
- _**Note**: If the list of commits is long, you will be able to use your up and down arrow keys to scroll through the log. After you are done viewing, you can hit `q` to exit this read mode._
  
Below is a sample output of `git log`:

```
commit 2e525e4b1c40f6cffb78438285a00cd7eed54ae0 (HEAD -> master)
Author: username <email@example.com>
Date:   Thu Apr 2 23:53:30 2020 -0700

    second commit

commit 8c20a14b99d7a490580045176287b979c93d9cb5
Author: username <email@example.com>
Date:   Wed Apr 1 22:49:52 2020 -0700

    initial commit

```

<br>

## `git diff`

**`git diff`**: Show changes between files, commits, etc.

<!--
FROM HELP FILE:

Show changes between commits, commit and working tree, etc.

Show changes between the working tree and the index or a tree, changes between the index and a tree, changes between two trees, changes between two blob objects, or changes between two files on disk
-->

- Help: `git diff --help`
- Syntax:
  - `git diff [<file_name(s)>]`: Show changes made to unstaged files in _working directory_ compared to the "index"
    - In other words, these are the changes that would get added to the _staging area_ if you `git add` them
    - This only applies to tracked files (i.e., files listed under `Changes not staged for commit` when you check `git status`), since untracked files have no history in the "index" to compare against
    - If no `file_name(s)` specified, `git diff` shows changes made to all tracked, unstaged files
  - `git diff --cached [<file_name(s)>]`: Show changes made to added files in _staging area_ compared to the last commit
    - In other words, these are the changes that would be committed if you run `git commit` command
    - If no `file_name(s)` specified, `git diff --cached` shows changes made to all staged files (i.e., files listed under `Changes to be committed` when you check `git status`)
    - If this is the initial commit, then all staged changes are shown
  - `git diff <commit_hash> <commit_hash> [<file_name(s)>]`: Show changes between the two specified commits
    - If no `file_name(s)` specified, `git diff <commit_hash> <commit_hash>` shows changes between all files
- Output: Comparison results for each file being checked by `git diff`
  - Each output starts with `diff --git a/<file_name> b/<file_name>`, which indicates that two versions of `file_name` is being compared
  - This is followed by some information about whether the versions are previously tracked by Git (indicated by `index`) or if a new file is involved (as in the case of `git diff --cached` for an untracked, staged file -- see second example below)
  - The line-by-line comparison of the file begins after the part in the output that starts with `@@`
    - A `-` in front of a line indicates that the line has been removed in `b/<file_name>` as compared to `a/<file_name>`
    - A `+` in front of a line indicates that the line has been added in `b/<file_name>` as compared to `a/<file_name>`
  
Below is a sample output of `git diff`:

```
diff --git a/create_dataset.R b/create_dataset.R
index c1cff38..5ea84e9 100644
--- a/create_dataset.R
+++ b/create_dataset.R
@@ -1,2 +1,2 @@
 library(tidyverse)
-mpg %>% head(5)
+mpg %>% filter(year == 2008)
```

<br>
<details><summary>**Example**: Checking `git diff` for an untracked file</summary>

- Imagine you have created a new file called `create_dataset.R` in your git repository
- Because this file has never been added to "index" before, you will not see any ouput to `git diff`


```bash
# Create new R script
echo "library(tidyverse)" > create_dataset.R

git diff  # No output
```
</details>

<br>
<details><summary>**Example**: Checking `git diff` for a staged file</summary>

- After staging `create_dataset.R`, it will be added to the "index"
- `git diff --cached` can be used to view all staged changes


```bash
# Add R script
git add create_dataset.R

git diff --cached
```

```
diff --git a/create_dataset.R b/create_dataset.R
new file mode 100644
index 0000000..8b151a2
--- /dev/null
+++ b/create_dataset.R
@@ -0,0 +1 @@
+library(tidyverse)
```
</details>

<br>
<details><summary>**Example**: Checking `git diff` for a modified, tracked file</summary>

- If you make further modifications to a file that’s being tracked (i.e., a file that’s been added before), you can use `git diff` to see changes between the versions in the _working directory_ and the _staging area_


```bash
# Modify create_dataset.R
echo "mpg %>% head(5)" >> create_dataset.R

git diff
```

```
diff --git a/create_dataset.R b/create_dataset.R
index 8b151a2..c1cff38 100644
--- a/create_dataset.R
+++ b/create_dataset.R
@@ -1 +1,2 @@
 library(tidyverse)
+mpg %>% head(5)
```
</details>

<br>
<details><summary>**Example**: Checking `git diff` after committing changes</summary>

- Suppose you commit the staged changes (i.e., the line `library(tidyverse)` in `create_dataset.R`)
- Note that the output of `git diff` (i.e., comparing changes between the _working directory_ and "index") is the same as the previous example, when the changes were just staged and not yet committed


```bash
# Make a commit
git commit -m "add 1st line to create_dataset.R"

git diff
```

```
diff --git a/create_dataset.R b/create_dataset.R
index 8b151a2..c1cff38 100644
--- a/create_dataset.R
+++ b/create_dataset.R
@@ -1 +1,2 @@
 library(tidyverse)
+mpg %>% head(5)
```
</details>

<br>
<details><summary>**Example**: Checking `git diff` between commits</summary>

- Now suppose we add the new changes made to `create_dataset.R` in the _working directory_ (i.e., the line `mpg %>% head(5)`) and make a second commit


```bash
# Add create_dataset.R and make a commit
git add create_dataset.R
git commit -m "add 2nd line to create_dataset.R"

git log
```

```
commit aa89efba9adddf8547b3743ba81a421dd2a28881 (HEAD -> master)
Author: cyouh95 <25449416+cyouh95@users.noreply.github.com>
Date:   Sat Apr 4 03:20:15 2020 -0700

    add 2nd line to create_dataset.R

commit d5c6e0958fb173af04f7e2c5d5fd81457e8ffd0c
Author: cyouh95 <25449416+cyouh95@users.noreply.github.com>
Date:   Sat Apr 4 03:11:38 2020 -0700

    add 1st line to create_dataset.R
```

- We can use `git diff` to check the differences between the two commits by specifying their hash ID's
- As seen below, the line `mpg %>% head(5)` has been added between the two commits


```bash
git diff d5c6e09 aa89efb
```

```
diff --git a/create_dataset.R b/create_dataset.R
index 8b151a2..c1cff38 100644
--- a/create_dataset.R
+++ b/create_dataset.R
@@ -1 +1,2 @@
 library(tidyverse)
+mpg %>% head(5)
```

- Note that the order we specify the commit hash ID's in matters
- As seen below, if we specify the ID of second commit and then the first commit, the displayed differences show that the line `mpg %>% head(5)` has been removed between the two commits


```bash
git diff aa89efb d5c6e09
```

```
diff --git a/create_dataset.R b/create_dataset.R
index c1cff38..8b151a2 100644
--- a/create_dataset.R
+++ b/create_dataset.R
@@ -1,2 +1 @@
 library(tidyverse)
-mpg %>% head(5)
```
</details>
<br>

# Git: Under the hood

## `.git/` directory

<br>
Every git repository that is created using `git init` contains a **`.git/` directory** that "contains all the informations needed for git to work" (From [Git series 1/3: Understanding git for real by exploring the .git directory](https://www.daolf.com/posts/git-series-part-1/)):


```bash
# Initialize a new git repository in `my_git_repo` directory
cd my_git_repo
git init

ls -al
```

```
## Initialized empty Git repository in /Users/cyouh95/my_git_repo/.git/
## total 0
## drwxr-xr-x    3 cyouh95  staff   102 Apr  4 22:45 .
## drwxr-xr-x+ 101 cyouh95  staff  3434 Apr  4 22:45 ..
## drwxr-xr-x    9 cyouh95  staff   306 Apr  4 22:45 .git
```

<br>
What's inside the **`.git/` directory**?


```bash
# List out the contents of the .git/ directory (in tree form)
find .git -print | sed -e 's;[^/]*/;|____;g;s;____|; |;g'
```

```
## .git
## |____config
## |____description
## |____HEAD
## |____hooks
## | |____applypatch-msg.sample
## | |____commit-msg.sample
## | |____post-update.sample
## | |____pre-applypatch.sample
## | |____pre-commit.sample
## | |____pre-push.sample
## | |____pre-rebase.sample
## | |____pre-receive.sample
## | |____prepare-commit-msg.sample
## | |____update.sample
## |____info
## | |____exclude
## |____objects
## | |____info
## | |____pack
## |____refs
## | |____heads
## | |____tags
```

We will be focusing on:

- `objects/`: Directory containing all git objects
- `HEAD`: Reference to the latest commit of the current branch
- `refs/`: Directory containing the hash ID of commit referred to by `HEAD`

We'll get into git objects starting in the next section, and see an example of `HEAD` and `refs/` in a [later section](#head-and-refs).

## Git objects

What is a **git object**?

- "A git repository is actually just a collection of objects, each identified with their own hash." (From [Deep dive into git: git Objects](https://aboullaite.me/deep-dive-into-git/))
  - A "hash" can be thought of as an unique ID that points to the git object
  - "Git is a simple key-value data store. You put a value into the repository and get a key by which this value can be accessed." (From [Becoming a Git pro. Part 1: internal Git architecture](https://indepth.dev/becoming-a-git-pro-part-1-internal-git-architecture/))
    - Key = Hash
    - Value = Git object
- Git objects are stored inside the `.git/objects` directory
  - The first 2 characters of its hash will be the name of the sub-directory within `.git/objects` that it is located in
  - The rest of the hash will be the git object filename
- The `git cat-file` command can be used to view information about a git object whose hash you specify

<br>
**`git cat-file`**: Provide content or type and size information for repository objects

- Help: `git cat-file -help`
- Syntax: `git cat-file [<option(s)>] <object>`
- Options:
  - `-p`: Pretty-print the contents of `<object>` based on its type
  - `-t`: Instead of the content, show the object type identified by `<object>`
  - `-s`: Instead of the content, show the object size identified by `<object>`

<br>
There are 4 types of **git objects** (From [The Git Object Model](http://shafiul.github.io/gitbook/1_the_git_object_model.html))
  
- [Blob](#blob-object)
- [Tree](#tree-object)
- [Commit](#commit-object)
- [Tag](#tag-object)

### Blob object

A **blob** is generally a file which stores data

- For example, this could be an R script
- The file must be added to the _staging area_ (i.e., "index") in order for the blob object to be created


```bash
# Create new R script
echo "library(tidyverse)" > create_dataset.R
echo "mpg %>% head(5)" >> create_dataset.R

# Add R script
git add create_dataset.R

# View .git/objects directory
find .git/objects -print | sed -e 's;[^/]*/;|____;g;s;____|; |;g'
```

```
## |____objects
## | |____c1
## | | |____cff389562e8bc123e6691a60352fdf839df113
## | |____info
## | |____pack
```

<br>
<details><summary>**Example**: Using `git cat-file` to view blob object content</summary>


```bash
# View content of create_dataset.R
git cat-file -p c1cff38
```

```
## library(tidyverse)
## mpg %>% head(5)
```
</details>

<br>
<details><summary>**Example**: Using `git cat-file` to view blob object type</summary>


```bash
# View content of create_dataset.R
git cat-file -t c1cff38
```

```
## blob
```
</details>

<br>
<details><summary>**Example**: Using `git cat-file` to view blob object size</summary>


```bash
# View content of create_dataset.R
git cat-file -s c1cff38
```

```
## 35
```
</details>
<br>

### Tree object

A **tree** is a directory that contains references to blobs (_files_) or other trees (_sub-directories_)

- Any sub-directories created inside the git repository is a tree object
  - It contains references to any blobs (_files_) or additional trees (_sub-directories_) within it
- The root directory of the git repository is also a tree itself, and contains references to all its content at the point of commit (like a "snapshot")
- A commit must be made in order for the tree object(s) to be created


```bash
# Create a sub-directory 
mkdir notes

# Add files to the sub-directory (since git doesn't track empty directories)
echo "This is my first set of notes." > notes/note_1.txt
echo "This is my second set of notes." > notes/note_2.txt

# Add new files
git add .

# View .git/objects directory
find .git/objects -print | sed -e 's;[^/]*/;|____;g;s;____|; |;g'
```

```
## |____objects
## | |____47
## | | |____6fb98775843929ca6c55b16b04752d973b3d2a
## | |____61
## | | |____08458417308ddc15d7390a2f8db50cf65ec399
## | |____c1
## | | |____cff389562e8bc123e6691a60352fdf839df113
## | |____info
## | |____pack
```

<br>
As seen, new blob objects are created for `note_1.txt` and `note_2.txt` since the files have been added (but tree objects will not be created until a commit has been made):


```bash
# View content of note_1.txt and note_2.txt
git cat-file -p 476fb98
git cat-file -p 6108458
```

```
## This is my second set of notes.
## This is my first set of notes.
```

<br>
After the files have been committed, tree objects will be created for any sub-directories as well as for the root directory of the repository:


```bash
# Make a commit
git commit -m "initial commit"

# View .git/objects directory
find .git/objects -print | sed -e 's;[^/]*/;|____;g;s;____|; |;g'
```

```
## [master (root-commit) 899e753] initial commit
##  3 files changed, 4 insertions(+)
##  create mode 100644 create_dataset.R
##  create mode 100644 notes/note_1.txt
##  create mode 100644 notes/note_2.txt
## |____objects
## | |____47
## | | |____6fb98775843929ca6c55b16b04752d973b3d2a
## | |____61
## | | |____08458417308ddc15d7390a2f8db50cf65ec399
## | |____6c
## | | |____f7bbf49af4f9fd5103cf9f0a3fa25226b12336
## | |____89
## | | |____9e7538c09e4989ce37882d56cb81a6fd6b27d7
## | |____c1
## | | |____cff389562e8bc123e6691a60352fdf839df113
## | |____f5
## | | |____9085df29aed7826a89b23af3f67fc3ab96f643
## | |____info
## | |____pack
```

<br>
As we now see, the tree objects for the `my_git_repo/` root directory and `notes/` sub-directory exists, and another object has been created for the commit (_more info on that in [next section](#commit-object)_):


```bash
# View object type for my_git_repo/ and notes/ trees
git cat-file -t f59085d
git cat-file -t 6cf7bbf

# View object type for the commit
git cat-file -t $(git rev-parse --short HEAD)  # git rev-parse retrieves latest commit hash
```

```
## tree
## tree
## commit
```

<br>
The content of a tree object is a list of all blobs (_files_) and other trees (_sub-directories_) in the directory. Each list entry follows the format:

```
<permission_code> <object_type> <object_hash> <object_name>
```

- `<permission_code>`: Code indicating who has read/write access to the object
  - This is typically `100644` for blobs and `100755` or `040000` for trees
- `<object_type>`: Type of the object (i.e., blobs or trees)
- `<object_hash>`: Reference to the object (i.e., the hash)
- `<object_name>`: Name of the file or directory

<br>
<details><summary>**Example**: Using `git cat-file` to view tree object content for `my_git_repo/` root directory</summary>


```bash
# View content of my_git_repo/ tree object
git cat-file -p f59085d
```

```
## 100644 blob c1cff389562e8bc123e6691a60352fdf839df113	create_dataset.R
## 040000 tree 6cf7bbf49af4f9fd5103cf9f0a3fa25226b12336	notes
```
</details>

<br>
<details><summary>**Example**: Using `git cat-file` to view tree object content for `notes/` sub-directory</summary>


```bash
# View content of notes/ tree object
git cat-file -p 6cf7bbf
```

```
## 100644 blob 6108458417308ddc15d7390a2f8db50cf65ec399	note_1.txt
## 100644 blob 476fb98775843929ca6c55b16b04752d973b3d2a	note_2.txt
```
</details>
<br>

### Commit object

A **commit** object is created after a commit is made that contains information about the commit:

```
tree <tree_hash>
parent <commit_hash>
author <username> <email> <time>
committer <username> <email> <time>

<commit_message>
```

- `tree`: Reference to the root directory tree object (i.e., "snapshot" of repository at the point of commit)
- `parent`: Reference to the parent commit
- Other information about the commit (e.g., `author`, `committer`, `commit_message`)


<br>
All commits except for the initial commit will contain a reference to its `parent` commit. So let's create a second commit:


```bash
# Modify R script
echo "df <- mpg %>% filter(year == 2008)" >> create_dataset.R

# Add R script
git add create_dataset.R

# Make another commit
git commit -m "second commit"

# View .git/objects directory
find .git/objects -print | sed -e 's;[^/]*/;|____;g;s;____|; |;g'
```

```
## [master 45f78a4] second commit
##  1 file changed, 1 insertion(+)
## |____objects
## | |____45
## | | |____f78a4af7aba974790ff42c2f10925543cc5ecd
## | |____47
## | | |____6fb98775843929ca6c55b16b04752d973b3d2a
## | |____49
## | | |____0ec1c138021b8d5c196c26a2a7b3de69afc2d1
## | |____52
## | | |____4db779f0a3e3b3b353b522285c7da4830e21f1
## | |____61
## | | |____08458417308ddc15d7390a2f8db50cf65ec399
## | |____6c
## | | |____f7bbf49af4f9fd5103cf9f0a3fa25226b12336
## | |____89
## | | |____9e7538c09e4989ce37882d56cb81a6fd6b27d7
## | |____c1
## | | |____cff389562e8bc123e6691a60352fdf839df113
## | |____f5
## | | |____9085df29aed7826a89b23af3f67fc3ab96f643
## | |____info
## | |____pack
```


<br>
<details><summary>**Example**: Using `git cat-file` to view commit object content for first commit</summary>

- _**Note**: The commit hash will be different each time we run this because it is dependent on the time_


```bash
# Retrieve commit hash for first commit
git rev-list HEAD | tail -n 1

# View content of the commit object
git cat-file -p $(git rev-list HEAD | tail -n 1)
```

```
## 899e7538c09e4989ce37882d56cb81a6fd6b27d7
## tree f59085df29aed7826a89b23af3f67fc3ab96f643
## author cyouh95 <25449416+cyouh95@users.noreply.github.com> 1586065548 -0700
## committer cyouh95 <25449416+cyouh95@users.noreply.github.com> 1586065548 -0700
## 
## initial commit
```
</details>

<br>
<details><summary>**Example**: Using `git cat-file` to view commit object content for second commit</summary>

- _**Note**: The commit hash will be different each time we run this because it is dependent on the time_


```bash
# Retrieve commit hash for lastest commit
git rev-parse HEAD

# View content of the commit object
git cat-file -p $(git rev-parse HEAD)
```

```
## 45f78a4af7aba974790ff42c2f10925543cc5ecd
## tree 524db779f0a3e3b3b353b522285c7da4830e21f1
## parent 899e7538c09e4989ce37882d56cb81a6fd6b27d7
## author cyouh95 <25449416+cyouh95@users.noreply.github.com> 1586065548 -0700
## committer cyouh95 <25449416+cyouh95@users.noreply.github.com> 1586065548 -0700
## 
## second commit
```
</details>
<br>

### Tag object

A **tag** object is created after a tag is generated:

```
object <object_hash>
type <object_type>
tag <tag_name>
tagger <username> <email> <time>

<tag_message>
```

- `object`: Reference to the tagged object
- `type`: Object type of the tagged object (usually a `commit`)
- Other information about the tag (e.g., name of `tag`, `tagger`, `tag_message`)

Let's create a tag for the current commit:


```bash
# Create a tag
git tag -a v1 -m "version 1.0"

# View .git/objects directory
find .git/objects -print | sed -e 's;[^/]*/;|____;g;s;____|; |;g'
```

```
## |____objects
## | |____36
## | | |____e85fa4c353b4a0f6e8192c5c2caf95a895bc36
## | |____45
## | | |____f78a4af7aba974790ff42c2f10925543cc5ecd
## | |____47
## | | |____6fb98775843929ca6c55b16b04752d973b3d2a
## | |____49
## | | |____0ec1c138021b8d5c196c26a2a7b3de69afc2d1
## | |____52
## | | |____4db779f0a3e3b3b353b522285c7da4830e21f1
## | |____61
## | | |____08458417308ddc15d7390a2f8db50cf65ec399
## | |____6c
## | | |____f7bbf49af4f9fd5103cf9f0a3fa25226b12336
## | |____89
## | | |____9e7538c09e4989ce37882d56cb81a6fd6b27d7
## | |____c1
## | | |____cff389562e8bc123e6691a60352fdf839df113
## | |____f5
## | | |____9085df29aed7826a89b23af3f67fc3ab96f643
## | |____info
## | |____pack
```

<br>
<details><summary>**Example**: Using `git cat-file` to view tag object</summary>

```bash
# View content of the tag object
git cat-file -p $(git show-ref -s v1)  # retrieves hash for v1 tag
```

```
## object 45f78a4af7aba974790ff42c2f10925543cc5ecd
## type commit
## tag v1
## tagger cyouh95 <25449416+cyouh95@users.noreply.github.com> 1586065549 -0700
## 
## version 1.0
```


```bash
# The tagged object was the second commit
git log
```

```
## commit 45f78a4af7aba974790ff42c2f10925543cc5ecd
## Author: cyouh95 <25449416+cyouh95@users.noreply.github.com>
## Date:   Sat Apr 4 22:45:48 2020 -0700
## 
##     second commit
## 
## commit 899e7538c09e4989ce37882d56cb81a6fd6b27d7
## Author: cyouh95 <25449416+cyouh95@users.noreply.github.com>
## Date:   Sat Apr 4 22:45:48 2020 -0700
## 
##     initial commit
```
</details>
<br>

## `HEAD` and `refs/`

Recall from earlier that `HEAD` is a reference to the latest commit of the current branch (in this case, _master_):


```bash
# View content of HEAD
cat .git/HEAD
```

```
## ref: refs/heads/master
```

The hash ID of the commit referred to by `HEAD` is located in the `refs/` directory:


```bash
# View content of refs/heads/master
cat .git/refs/heads/master
```

```
## 45f78a4af7aba974790ff42c2f10925543cc5ecd
```

We can use `git log` to verify that this is the hash ID of the latest commit:


```bash
# View commit log
git log
```

```
## commit 45f78a4af7aba974790ff42c2f10925543cc5ecd
## Author: cyouh95 <25449416+cyouh95@users.noreply.github.com>
## Date:   Sat Apr 4 22:45:48 2020 -0700
## 
##     second commit
## 
## commit 899e7538c09e4989ce37882d56cb81a6fd6b27d7
## Author: cyouh95 <25449416+cyouh95@users.noreply.github.com>
## Date:   Sat Apr 4 22:45:48 2020 -0700
## 
##     initial commit
```


# Git commands: Undoing changes

Below are some common git commands you might use to undo changes:

- [git checkout](#git-checkout)
- [git reset](#git-reset)
- [git revert](#git-revert)

## `git checkout`

**`git checkout`**: Restore working tree files (or switch branches)

- Help: `git checkout -help`
- Syntax: `git checkout [<file_name(s)>]`
- Result: Undo changes made to specified `file_name(s)` in the _working directory_
    - This only applies to tracked, unstaged files (i.e., files listed under `Changes not staged for commit` when you check `git status`)
- _**Note**: The `git checkout` command can also be used for switching branches, but that will be covered later_

<br>
<details><summary>**Example**: Using `git checkout` to discard changes to a tracked, unstaged file</summary>

- Imagine you have made some changes to a file called `create_dataset.R` since you last committed it
- You can use `git checkout` to discard these changes in the _working directory_


```bash
# Check status before discarding changes
git status
```

```
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   create_dataset.R

no changes added to commit (use "git add" and/or "git commit -a")
```


```bash
git checkout create_dataset.R

# Check status after discarding changes
git status
```

```
On branch master
nothing to commit, working tree clean
```
</details>
<br>


## `git reset`

**`git reset`**: Reset current `HEAD` to the specified state

- Help: `git reset -help`
- Syntax:
  - `git reset HEAD <file_name(s)>`: Unstages the specified `file_name(s)` from the _staging area_ to the _working directory_
    - This only applies to staged files (i.e., files listed under `Changes to be committed` when you check `git status`) and will move them back under `Changes not staged for commit` or `Untracked files`
    - `HEAD` is a pointer to the latest commit and will restore the "index" to that state
  - `git reset <commit_hash>`: Undo all commits up to (but not including) the specified `commit_hash`
    - The `HEAD` pointer will be set to the specified commit

<br>
<details><summary>**Example**: Using `git reset` to unstage a file</summary>

- Imagine you used `git add` to add `create_dataset.R` and `new_file.txt` to the _staging area_
- You can use `git reset` to unstage these files
  - Note that you can use `.` to specify all files in the currenty directory to be unstaged


```bash
# Check status before unstaging files
git status
```

```
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

	modified:   create_dataset.R
	new file:   new_file.txt
```


```bash
git reset HEAD .

# Check status after unstaging files
git status
```

```
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   create_dataset.R

Untracked files:
  (use "git add <file>..." to include in what will be committed)

	new_file.txt
```
</details>

<br>
<details><summary>**Example**: Using `git reset` to undo a commit</summary>

- Imagine you have made two commits adding lines to a file called `create_dataset.R`
- You can use `git reset` to undo commit(s) by specifying the hash ID of the commit you want to undo up to


```bash
# Check log before undoing commit
git log
```

```
commit 4a2f6fe315ff94aee5646c01a4e693e37b610119 (HEAD -> master)
Author: cyouh95 <25449416+cyouh95@users.noreply.github.com>
Date:   Sat Apr 4 13:04:35 2020 -0700

    add 2nd line to create_dataset.R

commit d5c6e0958fb173af04f7e2c5d5fd81457e8ffd0c
Author: cyouh95 <25449416+cyouh95@users.noreply.github.com>
Date:   Sat Apr 4 03:11:38 2020 -0700

    add 1st line to create_dataset.R
```


```bash
git reset d5c6e09

# Check log after undoing commit
git log
```

```
commit d5c6e0958fb173af04f7e2c5d5fd81457e8ffd0c (HEAD -> master)
Author: cyouh95 <25449416+cyouh95@users.noreply.github.com>
Date:   Sat Apr 4 03:11:38 2020 -0700

    add 1st line to create_dataset.R
```
</details>
<br>

## `git revert`

**`git revert`**: Revert existing commit(s)

- Help: `git revert -help`
- Syntax:
  - `git revert <commit_hash>`: Revert all commits up to and including the specified `commit_hash`
    - The difference between `git revert` and `git reset` is that the former does not completely remove all past commits, but creates a new one that reverts those changes
    - _**Note**: After entering this command, you'll be given a chance to edit the commit message of the new commit. Just enter `:q` to use the default message._

<br>
<details><summary>**Example**: Using `git revert` to revert a commit</summary>

- Imagine you have made two commits adding lines to a file called `create_dataset.R`
- You can use `git revert` to revert committed changes by specifying the hash ID of the unwanted commit


```bash
# Check log before reverting commit
git log
```

```
commit 4a2f6fe315ff94aee5646c01a4e693e37b610119 (HEAD -> master)
Author: cyouh95 <25449416+cyouh95@users.noreply.github.com>
Date:   Sat Apr 4 13:04:35 2020 -0700

    add 2nd line to create_dataset.R

commit d5c6e0958fb173af04f7e2c5d5fd81457e8ffd0c
Author: cyouh95 <25449416+cyouh95@users.noreply.github.com>
Date:   Sat Apr 4 03:11:38 2020 -0700

    add 1st line to create_dataset.R
```


```bash
git revert 4a2f6fe

# Check log after reverting commit
git log
```

```
commit f4dfac7d8703a94c4353eb27418646ca158792b4 (HEAD -> master)
Author: cyouh95 <25449416+cyouh95@users.noreply.github.com>
Date:   Sat Apr 4 13:10:41 2020 -0700

    Revert "add 2nd line to create_dataset.R"

    This reverts commit 4a2f6fe315ff94aee5646c01a4e693e37b610119.

commit 4a2f6fe315ff94aee5646c01a4e693e37b610119
Author: cyouh95 <25449416+cyouh95@users.noreply.github.com>
Date:   Sat Apr 4 13:04:35 2020 -0700

    add 2nd line to create_dataset.R

commit d5c6e0958fb173af04f7e2c5d5fd81457e8ffd0c
Author: cyouh95 <25449416+cyouh95@users.noreply.github.com>
Date:   Sat Apr 4 03:11:38 2020 -0700

    add 1st line to create_dataset.R
```
</details>
