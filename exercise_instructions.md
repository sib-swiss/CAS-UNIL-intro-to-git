# Exercises - Introduction to Git

## Before you start

* :pencil:
  **Exercise material setup:** download the `exercises.zip` archive file to
  your local computer and unzip it. This will unpack a directory named
  `exercises`, with all the data needed for the course exercises.

* :pencil2:
  **Additional Tasks:** at the end of exercises, you will sometimes find a
  section named **Additional Tasks**. These sections contain tasks to complete
  **if you have the time and after having completed the main exercise**.
  Additional tasks sections will not be corrected in class, but their solution
  is given in this document.

* :white_check_mark:
  **Exercise solutions:** all exercises and additional tasks section have
  their solution embedded in this document. Solutions are hidden by default,
  but you can reveal them by clicking on them. Here is an example:

  <details><summary><b>Solution (click to reveal)</b></summary>
  :sparkles: This reveals the answer :sparkles:
  </details>

  We encourage you to **not look at the solutions too quickly**, and try to
  solve the exercises without them. Remember that you can always ask the
  course teachers for help.

* :fire:
  **Tip:** if you are viewing these instructions on the GitHub web-interface,
  you can display a table of content (outline) of this page by clicking on the
  small icon that looks like a bulleted list near the top-right of this page.

### Configuring Git

Before starting with the exercises, make sure that you have done the minimal
Git configuration by setting your **user name** and **email** address in Git:

```sh
git config --global user.name "First-name Last-name"
git config --global user.email "your.email@example.com"

# Examples:
git config --global user.name "Alice Smith"
git config --global user.email alice.smith@wonder.org
```

Optionally you can also change the **default editor** used by Git, e.g. if you
are more comfortable with `nano` than `vim`:

```sh
git config --global core.editor nano  # Set default editor to nano.
git config --global core.editor vim   # Set default editor to vim (the default).
```

To see the config values that are currently set, the commands are the following:

```sh
git config --get user.name
git config --get user.email
git config --get core.editor

# Show all config values at once and where they are set.
git config --list --show-origin
```

<br>  
<br>

## Exercise 1 - Your first commit [30 min]

:rocket:
**Objective:** learn to create a new Git repository, add files to it, and
make commits.  

Welcome to the first exercise of this Git course! This is a warm-up, so you
will be guided step-by-step on exactly what you need to do.

### Part A: create a new repo from scratch and make a first commit

1. **Change directory** into `exercise_1/test-project` and list the
   directory's content using the following shell commands:
   ```sh
   cd exercise_1/test-project   # Enter the directory.
   ls -l                        # List files present the directory.
   ```
   You should see that it contains files reminiscent of a simple scripting
   project, e.g. a data analysis pipeline (here written in Python).
   ```txt
     test-project
      ├── doc
      │   └── user-guide.pdf
      ├── README.md
      ├── script.py
      ├── script.pyc
      └── tests
          ├── output.csv
          ├── tests.py
          └── tests.pyc
   ```
   <br>

2. **Initialize a new Git repository** at the root of the `test-project`
   directory:
   * Create a new Git repo in the current working directory with the command:
     ```sh
     git init   # Initialize a new Git repository.
     ```
   * Initializing a new Git repo creates a **hidden `.git` directory**. You
     can view this directory by running the shell command:
     ```sh
     ls -la   # You should observe that a new ".git" hidden directory was created.
     ```
   * :fire:
     **Important:** the **`.git`** directory is where Git stores the entire
     history of your repository (as well as various settings). If you delete
     it, all your version control history (and settings) for this repository
     **will be lost**. You can do this if e.g. you want to start the exercise
     from scratch again.

     <br>

3. **Display the status of files** in the working tree (i.e. the `test-project`
   directory):
   * Run the command `git status`.
   * :question:
     **Question:** what is the status of the files in your working directory?

   <details><summary><b>:white_check_mark: Solution</b></summary>

   **Running `git status`**, we see - as expected at this point - that
   **all files are untracked**.  
   Output of `git status`:

    ```txt
      On branch main
      No commits yet

      Untracked files:
        (use "git add <file>..." to include in what will be committed)
        README.md
        doc/
        script.py
        script.pyc
        tests/
    ```

   </details>
   <br>

4. **Stage the files** `README.md`, `script.py`, `doc/user-guide.pdf`,
   `tests/output.csv` and `tests/tests.py` (i.e. all files except the `*.pyc`
   files - these are "python cache" files we don't want to track).

   :owl:
   **Reminders:**
   * _staging_ a file is a synonym of _adding to the Git index_.
   * The command to stage files is: `git add <files or directories>`. For
     instance, `git add README.md` will stage the file `README.md`.

   To make sure that you have staged the files correctly, run the command
   **`git status`**. The output of the command should look like this:

      ```txt
      Changes to be committed:
          new file:   README.md
          new file:   doc/user-guide.pdf
          new file:   script.py
          new file:   tests/output.csv
          new file:   tests/tests.py

      Untracked files:
          script.pyc
          tests/tests.pyc
      ```

   <details><summary><b>:white_check_mark: Solution</b></summary>

   There are several ways we can stage all the requested files:
   * By staging individual files:
     ```sh
      git add README.md
      git add script.py
      git add doc/
      git add tests/tests.py
      git add tests/output.csv

      # Note that we can also stage all 3 files in a single command:
      git add README.md script.py doc/ tests/tests.py tests/output.csv
     ```
   * By staging all files in the directory, then removing the `*.pyc` files
     from the staging area (Git index):
     ```sh
     # Stage all files in the directory.
     git add --all   # Alternative: git add .

     # Remove the *.pyc files from the staging area.
     # Do not forget the --cached option, otherwise files are deleted on disk!
     git rm --cached script.pyc tests/tests.pyc 
     ```
     Note that in this specific case we can not use `git restore --staged` to
     remove files from the Git index because we do not have any commits yet
     in the repository (and `git restore --staged` needs a commit to restore
     from).

   If we now run `git status`, we should see that all our files except the
   `.pyc` files are displayed as "new file" under "Changes to be committed:",
   and are ready to be committed.

   </details>
   <br>

5. **Add a first commit** to the repo with the commit message
   `Initial commit for test project`.
   * The command to make a commit is: **`git commit -m "commit message"`**
     (or, in the long form: `git commit --message "commit message"`)
   * After the commit is done, run the following commands:  
     * **`git log`**: to display the repository's history. At this point you
       should have a single commit, that looks like the following (your exact
       values for commit hash, date, etc. will differ):
       ```txt
       commit a0da6303e9d6dfc34986f959a076721e153f382d (HEAD -> main)
       Author: Your Name <your.email@example.org>
       Date:   Mon Oct 14 13:55:08 2024 +0200

           Initial commit for test project
       ```
     * **`git show`**: to have a look at the content of your commit.  
     * :pushpin:
       **Note:** when the amount of text to be printed by `git show` exceeds one
       screen, the content is shown with the GNU program `less`. In `less`, you
       can use your keyboard arrow keys to move up/down, and press `q` to exit and
       return to the shell.

   :question:
   **Question:** why are the details of the `doc/user-guide.pdf` file not
   displayed by `git show`?

   <details><summary><b>:white_check_mark: Solution</b></summary>

    ```sh
    git commit -m "Initial commit for test project"
    git log     # At this point the commit history contains a single commit.
    git show    # Show content of the last commit.
    ```

    Looking at the output of **`git show`**, we can see that the newly
    added content for the file `doc/user-guide.pdf` is not displayed
    (unlike for `README.md` for instance, where the content of the file is
    shown).  
    The reason is that `user-guide.pdf` is a **binary file** and not a
    plain text file). Git does not display details for binary files.

   </details>

<br>

### Part B: commit a change to a tracked file

In this section, we will make an update to the `README.md` file, and then
create a new commit that adds the change we made.

1. **Open the `README.md` file** in your favorite text editor.
   * Change the 3rd line of the file to:
     > Demo project for the Git course. This will be great!
   * Save your changes and close the file.
   * Run **`git status`** - `README.md` should now be listed as modified:
      ```txt
      Changes not staged for commit:
          modified:   README.md

      Untracked files:
          script.pyc
          tests/
      ```
   <br>

2. **Display the changes** to files in the working tree:
   * Run the command **`git diff`**, which displays the difference in tracked
     files between the working tree and the Git index (staging area).
     ```sh
     git diff
     git diff README.md  # Gives the same result, as only README.md was modified.
     ```
   * It will show that `README.md` has one line removed (shown in red, prefixed
     with **`-`**), and one line added (shown in green, prefixed with **`+`**).
     ```diff
     -Demo project for the Git course.
     +Demo project for the Git course. This will be great!
     ```
   <br>

3. **Commit the changes** you just made:
   * Add/stage the changes made to `README.md` with **`git add`**.
     Remember that each time you modify a file and want to include these
     changes into your next commit, you have to `git add` that file again.
   * :fire:
     **Tip:** to stage all modified files at once, you can use the shortcut
     **`git add -u`**. Here it does not make a lot of difference as there is
     only 1 modified file, but if there are many of them, this command can be
     useful.
   * Run `git status` again: you should see that the `README.md` file is now
     listed under `Changes to be committed:` (in green).
   * Commit your changes with the message `"Make README file more cheerful"`.

    <details><summary><b>:white_check_mark: Solution</b></summary>

      ```sh
      git add README.md
      git commit -m "Make README file more cheerful"

      # Alternative: stage all modified files with "git add -u". Since only
      # README.md was modified, this is in the same as staging the README.md.
      git add -u
      git commit -m "Make README file more cheerful"

      # Alternative: use a "git commit" shortcuts to stage + commit in a single
      # command.
      git commit -m "Make README file more cheerful" README.md
      git commit --all -m "Make README file more cheerful"
      ```

    </details>

<br>

### Part C: adding files to the `.gitignore` list

At this point, the only files that should be left **untracked** in our
repository are the two `*.pyc` files (you can verify this by running
`git status`). Since we are never going to track these files, we would like
to **permanently ignore** them, so that they stop being listed as _untracked_.

1. At the root of the working tree, **create a text file named `.gitignore`**,
   with the following content:

    ```txt
    *.pyc
    ```

   :fire:
    **Tip**: you can create the `.gitignore` in any text editor you like,
    but you can also easily generate it with a shell command:

     ```sh
     echo "*.pyc" > .gitignore
     ```

    <br>

2. **Run `git status`**: you should see that you still have an untracked
   file: the `.gitignore` file you just created :sweat_smile: !  
   Since the ignore rules defined in the `.gitignore` file are useful to all
   users of the repository, this file should be added to the repo.

   * Stage the `.gitignore` file.
   * Make a new commit with commit message `Add *.pyc to ignore list`.
     At the end of this step, your working tree should now be "clean": when you
     run `git status`, the output should be:
      ```txt
        On branch main
        nothing to commit, working tree clean
      ```

   <details><summary><b>:white_check_mark: Solution</b></summary>

     ```sh
     # Stage the .gitignore file.
     git add .gitignore

     # Make a new commit.
     git commit -m "Add *.pyc to ignore list"

     # The working tree is now clean.
     git status  # -> nothing to commit, working tree clean
     ```

   </details>
   <br>

3. **Display the (modest) history of your Git repo** with the following
   variations of the **`git log`** command. Observe how history is displayed by
   each command:

    ```sh
    git log                                     # Prints the full commit message along with author and date.
    git log --pretty=oneline                    # 1 commit per line. Full commit hash/ID (checksum).
    git log --oneline                           # 1 commit per line. Abbreviated commit hash/ID.
    git log --all --decorate --oneline --graph  # Shows commits for all branches.
    ```

    With the current history of our git repo, the output of
    `git log --all --decorate --oneline --graph` is the same as
    `git log --oneline`. This will however change when we start working with
    **branches** - the longer version of the command will then become very
    useful.

    <br>

4. **Create a Git alias (shortcut)** for the command
   `git log --all --decorate --oneline --graph`, and name it `adog`.

    ```sh
    git config --global alias.adog "log --all --decorate --oneline --graph"
    ```

    * Test your new shortcut by typing: `git adog`.
    * Your commit history should look like this (commit ID values will differ):
      ```txt
      * 81d03aa (HEAD -> main) Add *.pyc to ignore list
      * 029a389 Make README file more cheerful
      * da59f94 Initial commit for test project
      ```
    * :fire:
      **Tip:** to list your Git aliases, use: `git config --list | grep ^alias`

<br>

### Additional Tasks (if you have time)

For some of the next steps, we will need an additional file named
`personal_notes.md`, as well as a change in the `script.py` file.

**Let's generate this file/changes** by running the following commands at the
root of the `test-project` directory:

```sh
echo "Let's keep this local" > personal_notes.md
echo "adding a bad line..." >> script.py

# You can then visualize the changes by running:
git status
git diff
```

<br>

#### 1. Removing content from the Git index (unstaging)

Start by staging all untracked and modified files with `git add --all`.
The status of your files should look like:

```txt
Changes to be committed:
    new file:   personal_notes.md
    modified:   script.py
```

Actually, we do _not_ want to add these changes to the repository,
so **let's unstage them**.

* Unstage the changes to `script.py` using the command: **`git restore --staged`**

* Unstage the entire file `personal_notes.md`, using either
  **`git restore --staged`** or **`git rm --cached`**.
  * :owl:
    **Reminder:** the difference between `git rm --cached` and `git restore --staged`
    is that `git rm --cached` **removes the entire file from the index**,
    while `git restore --staged` **reverts it to the version in the last
    commit** (`HEAD` commit).
  * :pushpin:
    **Note**: The reason why in this particular case `git rm --cached` does
    exactly the same as `git restore --staged` is because
    `personal_notes.md` is a newly added file. There is thus no difference
    between removing it completely, or just resetting it back to its
    version from the latest commit (since it is absent from the latest
    commit).
  * :warning:
    Be careful to _not_ run `git rm` instead of `git rm --cached`, as this
    would not only remove the file from the Git index, but also delete it
    from your working tree!

* At this point, changes in `script.py` should again be unstaged, and
  `personal_notes.md` should be untracked.  
  Run **`git status`** to confirm this:

  ```txt
  Changes not staged for commit:
    modified:   script.py

  Untracked files:
    personal_notes.md
  ```

<br>
<details><summary><b>:white_check_mark: Solution</b></summary>

```sh
  git add --all
  git restore --staged script.py           # Unstage changes to script.py
  git restore --staged personal_notes.md   # Unstage personal_notes.md

  # To unstage personal_notes.md, we can also use:
  git rm --cached personal_notes.md
```

</details>
<br>

#### 2. File staging shortcuts: `git add --update` vs. `git add --all`

:owl:
**Reminder:**

* **`git add --all`**: updates the Git index with **all modified and untracked files**.
* **`git add --update`:** updates the Git index only with the new versions
  of files that are **already tracked**. It does _not_ stage any new,
  untracked files. In a sense, `--update` is safer because it prevents you
  from adding completely new files to the Git repo by mistake.

In the task just above, we have used `git add --all`. Now we would like to
stage only the modified file `script.py`.

* Run the command **`git add -u`**, then look at the status of your files.
  You should see that only `script.py` was staged (because it's a modified
  file), but not `personal_notes.md` (because it's untracked).

  ```sh
  git add -u   # -u is the shortcut for --update
  git status
  ```

  ```txt
  Changes to be committed:
    modified:   script.py

  Untracked files:
    personal_notes.md
  ```

* Run `git restore --staged script.py` to unstage the changes to `script.py`.

<br>
<details><summary><b>:white_check_mark: Solution</b></summary>

The difference between **`git add -u/--update`** and **`git add --all`** is
that **`--update`** only adds files that are already tracked in Git, while
**`--all`** adds all files (except ignored files), whether they are already
tracked (modified) or not (untracked).

In a sense, `--update` is safer because it prevents you from adding
completely new files to the Git repo by mistake.

```sh
  # Stage all modified files.
  git add -u

  # We can see that modifications in script.py are now staged.
  git status

  # Unstage the modifications.
  git restore --staged script.py
```

</details>
<br>

#### 3. Ignore a file using `.git/info/exclude`

`personal_notes.md` is a file that we never intend to track and share with
other people. Therefore we would like to **ignore** it. However, since this
file is specific to our own local setup, it should only be ignored by our
local Git repo, and not by everyone else.

:owl:
**Reminder:** in Git, files/patterns to ignore only in your local repo
should be added to **`.git/info/exclude`**. This is a text file that is
automatically present in a `.git` repo.

* Edit the file **`.git/info/exclude`** to ignore `personal_notes.md`.
  you can do this with a regular text editor, or using the following
  shell command:

    ```sh
      echo "personal_notes.md" >> .git/info/exclude

      # Display the content of the file:
      cat .git/info/exclude
    ```

* Run `git status` again. The file `personal_notes.md` should not longer
  be listed as _untracked_.

<br>
<details><summary><b>:white_check_mark: Solution</b></summary>

```sh
  # Add 'personal_notes.md' to the "exclude" file:
  echo "personal_notes.md" >> .git/info/exclude
  cat .git/info/exclude  # Display the content of the file.

  # 'personal_notes.md' is no longer listed as untracked.
  git status
```

</details>
<br>

#### 4. Undo a changes from `script.py` in your working tree

If you run **`git diff`**, you will see that we currently have an
uncommitted change in the `script.py` file:

```diff
+adding a bad line...
```

However, this is not a modification we want to keep. Instead, we would
like to **reset the content of `script.py`** to its previous version
(as in the Git index and the previous commit).

* Using `git restore`, reset the content of `script.py` to its version in
  the Git index.
* Run `cat script.py` to make sure the  "bad line" has been removed from the
  file.
* Run `git diff`: there should be no difference anymore (no output).
* Run `git status`: at this point, your working tree should be clean.

  ```txt
  On branch main
  nothing to commit, working tree clean
  ```

:warning:
As you have just experienced, `git restore <file>` really overwrites
uncommitted modifications in your files. Use this command carefully to
avoid losing work by mistake.

<br>
<details><summary><b>:white_check_mark: Solution</b></summary>

**`git restore script.py`** overwrites the version of `script.py` present in
the working tree with the version from the Git index.

```sh
git restore script.py
cat script.py           # The line "adding a bad line..." is gone.
git diff                # Empty output, which is what we expect.
git status              # No more uncommitted changes.
```

</details>
<br>

#### 5. Remove a file from the repository

Currently the file `tests/output.csv` is being tracked in our Git repo.
However, all things considered, this file is not really needed, and we now
would like to delete it from both our repo and working tree.

* Delete the file with **`git rm`**.
* Run `git status`: you should see that the was was deleted, and that this
  deletion is already stage.
  ```txt
  On branch main
  Changes to be committed:
    deleted:    tests/output.csv
  ```
* Make a new commit that removes this file from the repo. You can use the
  commit message `"Remove test output"`

:owl:
**Reminder:** while we have delete the file `output.csv` from the git index
and the last commit, a copy of it still remains in the history of our
repository.

<br>
<details><summary><b>:white_check_mark: Solution</b></summary>

```sh
git rm tests/output.csv
git status
git commit -m "Remove test output"
```

We use **`git rm`** to remove `tests/output.csv` from both the Git index
and the working tree. To delete the file only from the index we would use
`git rm --cached tests/output.csv`.

</details>
<br>

#### 6. Retrieve `output.csv` from an older commit

Let's imagine that, for some reason, we want to retrieve the file
`tests/output.csv` from our commit history.

* Use command **`git restore --source <commit ref> tests/output.csv`**.
  You need to replace `<commit ref>` with the commit ID of the commit
  from where to retrieve the file.
* :fire:
  **Tip:** you can use `HEAD~1` to refer to the second-to-last commit.

<details><summary><b>:white_check_mark: Solution</b></summary>

The **`--source`** argument is used to indicate from which commit the file
should be restored. You can pass a commit ID (hash), or use a reference to
a commit such as `HEAD~1` in the solution below. `HEAD~1` refers to the
parent of the current `HEAD`.

```sh
git restore --source HEAD~1 tests/output.csv
```

</details>

<br>
<br>
<br>

## Exercise 2 - The Git reference web page  [30 min]

:rocket:
**Objective:** learn to use a basic branched workflow.

Well done! Your burgeoning Git skills have landed you a job as a junior
web-developer at _Scamazone Inc._, where you have been assigned the gratifying
task of fixing bugs in their website.

Let's get started:

1. Change directory into `/exercise_2`. You will see that it already contains
   a Git repository, as well as an HTML page named `references.html`.  
2. Open the `references.html` page in your web browser.
3. Explore the content of the Git repo using the `git log`, `git status` and
   `git branch` commands.

:question:
**Questions:**

* How many commits have already been made in the repo?  
* How many branches are present in the repo? How are they named?  
* Are there any uncommitted changes?  

<details><summary><b>:white_check_mark: Solution</b></summary>

```sh
cd exercise_2/
git log
git log --oneline  # There are 3 commits in the repo.
git branch         # There is currently only 1 branch: main.
git status         # There is one tracked file with uncommitted changes: references.html
```

</details>
<br>

### A) Fix the broken "ProGit" link

Your first task is to fix the broken link to the "ProGit" book in the HTML
page (currently when you click on the "ProGit" link with the webpage open in
your browser, it returns an error).

Since **we want to follow good practices**, we will _not_ work on this fix in
the **`main` branch**. Instead we will create a new branch, fix the link
problem on that branch, test our fix, and then merge it into `main` once we
are confident the problem is solved.

The reason we proceed in this way is because `main` is the branch that is used
to generate the **production version** of the Scamazone website (the version
that customers can see), and we don't want to make changes to that production
version until we are really sure that the changes we introduce in the code are
working as expected.

1. Before working on our fix in a new branch, we need to
   **make sure that our working tree is clean**:
    * Check the Git repo to see if there are any uncommitted changes.
    * If there are, display the uncommitted changes to see what they do. Even
      if you are not familiar with HTML, it should be easy enough to figure-out
      what the changes do.

2. Now that you have figured-out what the uncommitted changes do, stage the
   changes and **make a commit with a meaningful message**.  
   Verify that your working tree is now clean.

3. **Create a new branch named `fix`** and switch to it. This `fix` branch is
   where we will work on our bug fix, so that our changes to the code base
   remain isolated from the `main` branch until we are sure our fix is fine.

4. On the new branch, edit the `references.html` file to fix the link to the
   "ProGit" book.  
   :dart:
   **Hint:** to fix the link, add `https://` in front of the URL.  

5. **Verify in your browser** that the link is now working properly (you might
   have to reload the page).  
   If it is the case, you can commit your changes.
   Please **use a meaningful commit message**.

6. Now that our bug fix is production ready, **merge the `fix` branch into `main`**,
   then run `git log --all --decorate --oneline --graph` (or the `git adog`
   command if you have created this alias). At this point, the output should
   look like this
   (commit ID values will differ and your commit messages may be different):

   ```txt
    * 50a2e7f (HEAD -> main, fix) Fix broken ProGit link
    * e6a6176 Add Git logo placeholder to the Git reference webpage
    * 8a7444c Add Git logo file
    * c99fb57 Add links to Git resources
    * 5b54605 First commit. Add template for the Git reference page
   ```

7. **Delete the `fix` branch** as it is no longer needed.  
   Run `git branch` and/or `git log --all --decorate --oneline --graph` to make
   sure the `fix` branch was deleted.  

Enjoy your Git reference page. You can have a look at the different links if
you want to learn everything about Git!

<br>
<details><summary><b>:white_check_mark: Solution</b></summary>

1. Check if the working tree is clean, and see uncommitted changes.

   ```sh
    git status   # This shows that the references.html file has uncommitted changes.
    git diff     # Display the uncommitted changes in the file.
   ```

2. Commit the changes.

    ```sh
     git add references.html  # Stage the changes in the references.html file.
     git commit -m "Add Git logo placeholder to the Git reference webpage"

     # You can also use these shortcut for the above 2 lines:
     git commit -m "Add Git logo placeholder to the Git reference webpage" references.html
     # or
     git commit -am "Add Git logo placeholder to the Git reference webpage"

     git status  # There are no more uncommitted changes.
    ```

3. Create a new "fix" branch and switch to it.

    ```sh
     git branch fix
     git switch fix

     # You can use the following shortcut to create + switch to a branch in
     # a single command:
     git switch -c fix
    ```

4. Edit the HTML page, then verify in the browser that the link now works.
   You can use any text editor to do this.

    ```sh
    vim references.html
    ```

5. Make a commit with the changes:

    ```sh
     # Stage your changes (i.e. add changes to the Git index).
     git add references.html
     # Make a commit.
     git commit -m "Fix broken ProGit link"

     # Here are shortcuts for the above 2 lines:
     git commit -m "Fix broken ProGit link" references.html
     # or
     git commit -am "Fix broken ProGit link"
    ```

6. Merge `fix` into `main`.
   Note that no additional commit is created by the merge, because this is a
   "fast-forward" merge.

    ```sh
     git switch main
     git merge fix    

     # Display repo commit history.
     git log --all --decorate --oneline --graph
    ```

7. Delete the `fix` branch.

    ```sh
     git branch -d fix

     # Verify that the "fix" branch is gone.
     git branch

     # Show repo history again.
     git log --all --decorate --oneline --graph
    ```

</details>
<br>

### B) Additional Tasks (if you have time): add an image and new links

In this second part of the exercise, you are tasked to add a couple of new
links to books on the Git reference page.  
Here is what you should do:

1. Work in a **new branch** named `dev`.  

2. **Make a commit** that adds the following 2 references at the end of the
   list in the HTML page:  
   > `<li><a href="https://www.manning.com/books/learn-git-in-a-month-of-lunches">Learn git in a month of lunches</a></li>`  
   > `<li><a href="https://www.amazon.com/Git-Porch-Willie-Crawford-2006-02-01/dp/B01K95YGYG">Git Porch</a></li>`

    * Use **`git diff`** and **`git diff --cached`** to look at your edits in
      the HTML file, before and after staging them.
    * :question:
      **Question:** what is the difference between `git diff` and
      `git diff --cached`?
    * Check whether you did a proper job by refreshing the HTML page in your
      browser _before_ you commit your changes.  
   <br>

3. **Make a commit** that adds the Git logo to the webpage.
    * Replace the placeholder line that starts with `<!-- Add Git logo placeholder`
      by `<img src="git_logo.png">` in the HTML file.
    * :pushpin:
      **Note:** check whether you did a proper job by refreshing the HTML page
      in your browser _before_ you commit your changes.

4. When you have added these new features - and tested that they actually work
   by reloading the `references.html` page in your browser (new links are
   working, logo was added) - merge your `dev` branch into `main`.

5. **Run `git log --all --decorate --oneline --graph`** to display your entire
   repository's history. It should look like this:  
   (commit ID values will differ and commit message may be different)

   ```txt
    * ba4687a (HEAD -> main, dev) Add git logo
    * 30b149a Add two new books to Git reference page
    * 50a2e7f Fix broken ProGit link
    * e6a6176 Add Git logo placeholder to the Git reference webpage
    * 8a7444c Add Git logo file
    * c99fb57 Add links to Git resources
    * 5b54605 First commit. Add template for the Git reference page
   ```

6. **Delete the `dev` branch**, you no longer need it.  
   Verify it was deleted by running `git branch` and/or
   `git log --all --decorate --oneline --graph`.

<br>
<details><summary><b>:white_check_mark: Solution</b></summary>

Add new links to the web page:

```sh
# 1. Create and switch to a new "dev" branch.
git switch -c dev

# 2. Add the new references to the web page.
vim references.html     # Edit HTML page to add the new links...
git diff
git diff --cached
# After having checked that the two new links are working, stage and commit the changes.
git add references.html
git diff
git diff --cached
git commit -m "Add two new books to Git reference page"
```

:question:
**Question answer:** the difference between `git diff` and `git diff --cached`
is that the former will display the difference between the working tree
(files on disk) and the git index, while the later shows the difference
between the git index and the latest commit.

Add a Git logo to web page:

```sh
# 3. Edit the HTML page to add logo.
vim references.html
git commit -m "Add git logo" references.html
```

Merge changes into `main`, delete branch `dev`:

```sh
# 4. Merge "dev" into "main"
git switch main  # To merge "dev" into "main", we must be on the "main" branch.
git merge dev

# 5. Verify that both "dev" and "main" now point to the same commit.
git log --all --decorate --oneline --graph

# 6. Delete the "dev" branch.                             
git branch -d dev
```

</details>

<br>
<br>
<br>

## Exercise 3 - The markdown cheat-sheet  [30 min]

:rocket:
**Objectives:**

* Create a repo on GitHub.
* Practice the basic commands to interact with a remote: `git push`,
  `git pull`, and `git fetch`.

<br>

Good work so far! Your Git skills are improving and it's now time to start
working with **remote repositories**.

In this exercise, we will work on a small - and incomplete - cheat-sheet for
the **[Markdown language](https://www.markdownguide.org) syntax**. As you
probably sensed, this is a project of prime importance, and therefore we will
want to setup a **remote repository** for the project on GitHub, so that we
can i) have a backup of our work on GitHub, and ii) make it available to
everyone out there.

<br>

### A) Creating a new repo on GitHub

1. In your web browser, connect to your GitHub account and
   **create a new repository** with the following characteristics:
   * **Repository name:** `test-project`
   * **Repository description:**: `Test repository to learn working with remotes`
   * Visibility: **public** (anyone has read access).
   * Check the box: [x] Initialize this repository with a README.

   :dart:
   **Hint:** if needed, you can find instructions on how to create a new
   repository in the course slides.
   <br>

2. On your computer, enter the directory `exercise_3/` and
   **clone your new repository**. Enter the directory you just cloned: you
   should see that all it contains is a `README.md` file.
   <br>

3. **Display the content** of the README file with the command: `cat README.md`.

<br>
<details><summary><b>:white_check_mark: Solution</b></summary>

```sh
# After having created the new project on GitHub, clone the repository.
#  -> The exact URL of the project depends on your user name and the name
#     of the project on GitHub.
git clone https://github.com/<user name>/test-project.git

# Enter the directory and list its content: there is only 1 file, README.md
cd test-project
ls -l

# Display the content of the README file.
cat README.md
```

</details>
<br>

### B) Push commits to the remote

Let's now modify the content of the `README.md` file and push those changes
to the remote.

1. **Change the content of `README.md`** to the following text (you can copy
   the text using the icon on the top-right of the text block).

    ```md
    # A short primer to markdown syntax ! :dizzy:

    Markdown is a lightweight markup language that you can use to add
    formatting to plaintext text documents. Markdown is one of the most
    popular markup languages.

    Markdown files (files with a `.md` extension) such as this README file
    are automatically rendered by GitHub.
    ```

    <br>

2. **Run `git diff` and `git status`** to display changes you made to
   `README.md`.  
   Then **make a new commit** with your changes (use a meaningful commit
   message).

   <br>

3. **Display the history** of your repo with the command
   **`git log --all --decorate --oneline --graph`** (or use the **`git adog`**
   alias, if you created it). You should have 2 commits (commit ID values will
   differ):

    ```sh
    * 65efd2c (HEAD -> main) Update title and description of markdown guide
    * 88f9dea (origin/main, origin/HEAD) Initial commit
    ```

   :fire:
   **What you should pay attention to** is the respective positions of the
   **`main`** and **`origin/main`** branches:
   * **`main`** (in green) shows the
     **position of the `main` branch in our local repo**. It is pointing
     to the 2nd commit, the one we just added (`65efd2c` in the example above).
   * **`origin/main`** (in red) shows the
     **position of the `main` branch on the remote**. `origin/main` is still
     pointing to the initial commit (`88f9dea`), because we have
     **not pushed our new changes (commits)** to the remote.
   * :pushpin:
     **Note:** to be completely accurate, we should say that `origin/main`
     shows the **last known position of the `main` branch** on the remote.
     Remember that synchronization between a local and remote repo is _not_
     automatic.

   In this situation, the local `main` branch is said to be **ahead** of the
   remote. In other words, the new commit we just made (`65efd2c`) is only
   present on our local computer: if we lost access to our computer just now
   (e.g. it gets stolen while we enjoy one too many beers at the bar), we would
   have permanently lost the work we did in that last commit.

   :sparkles:
   **Tip:** running the command **`git status`** also warns us about the
   discrepancy between `main` and `origin/main` - see the 2nd line of the
   output below:

    ```sh
    On branch main
    Your branch is ahead of 'origin/main' by 1 commit.
      (use "git push" to publish your local commits)

    nothing to commit, working tree clean
    ```

    <br>

4. **Push your changes on `main` to the remote**, then display the history of
   your repo again. You should now see that both `main` and `origin/main` point
   to the latest commit.

    ```sh
    * 65efd2c (HEAD -> main, origin/main, origin/HEAD) Update title and description of markdown guide
    * 88f9dea Initial commit
    ```

    Likewise, running **`git status`**, now tells us that our local `main` is
    up to date with the remote (2nd line of the output below).

    ```sh
    On branch main
    Your branch is up to date with 'origin/main'.

    nothing to commit, working tree clean
    ```

    <br>

5. At this point, our **local and remote repositories are perfectly in sync**
   (all commits that we have locally are also present on the remote and
   vice-versa).
   To convince yourself that this is indeed the case, go to your project home
   page on GitHub: you will see that the updated version of the `README.md`
   file is displayed.

<br>
<details><summary><b>:white_check_mark: Solution</b></summary>

```sh
# 1-2. Modify the content of README.md (using any text editor) and visualize
#      the changes.
git status
git diff

# 3. Commit the changes and visualize the repo's history.
git add README.md
git commit -m "Update title and description of markdown guide"
git log --all --decorate --oneline --graph   # Or 'git adog'.
git status
# Alternatively, we could also stage and commit in a single command.
git commit -m "Update title and description of markdown guide" README.md

# 4. Push the new commit on `main` to the remote.
git push
# The remote origin/main is now up-to-date with the local `main` branch.
git log --all --decorate --oneline --graph
git status
```

</details>
<br>

### C) Pull changes from the remote

In this exercise you are working on your project alone - no one else is pushing
changes to your remote. Therefore, to simulate content being added to the
remote, we will use a small trick: we will add a commit to our repo via the
GitHub **web interface**.

1. **Go to the home page** of your project on GitHub and click on the
   **Edit file** button (the a small pencil icon displayed at the top-right
   of the `README.md` file - make sure you are signed-in).

2. The `README.md` file is now in edit mode:
   **copy-paste the following content at the end of the file** (do not remove
   what is already in the file, just add to it):

    ```md
    ## Bold and italic text

    * To render text **in bold**, surround it with `**` or `__`.  
      Example: `**this is important**` ---> **this is important**

    * To render text _in italic_, surround it with `_` or `*`.  
      Example: `this is *really great*` ---> this is _really great_

    ## Titles

    To create a title in markdown, add one (or more) `#` signs at the start
    of the line:
    * `#` = level 1 title (largest font).
    * `##` = level 2 title.
    * `###` = level 3 title.
    * `####` = level 4 title.
    * `#####` = level 5 title.
    * `######` = level 6 title (smallest font).

    Examples:
    ### This is a level 3 title...
    ##### This is a level 5 title...
    ```

   Then click on the green **"Commit changes..."** button and commit your
   changes to the `main` branch with the commit message:

   > Add bold, italic and titles to markdown guide

   After the changes are committed, you will see that the README file on
   your project's home page on GitHub gets updated with the new content.

   <br>

3. At this point, the new changes are **only present on the remote**. So let's
   sync our local copy of the repo with the new content from the remote.

   **Run `git fetch`:** this **retrieves (downloads) all new content** from
   the remote to our local repo. However, it will _not_ update the local
   `main` branch. To verify this, display your repo history, which should
   look like this:

    ```sh
    * efe768e (origin/main, origin/HEAD) Add bold, italic and titles to markdown guide
    * 65efd2c (HEAD -> main) Update title and description of markdown guide
    * 88f9dea Initial commit
    ```

   :sparkles:
   **Note the following:**
   * **The new commit** we made on the remote (`efe768e` in the example below)
     **has been downloaded to our local repo**: the data is now present on our
     computer and our history has 3 commits.
   * However, our local `main` branch is still pointing at the 2nd commit
     (`65efd2c`) - the new commit **was _not_ merged** into the local `main`.
   * You can try to run the command `cat README.md` to display the content of
     the README file: you will see that it does _not_ contain the new text that
     we added on GitHub.
   <br>

4. **Let's update our local `main` branch**: run the command:  **`git pull`**,
   then display your repo history again:

    ```sh
    * efe768e (HEAD -> main, origin/main, origin/HEAD) Add bold, italic and titles to markdown guide
    * 65efd2c Update title and description of markdown guide
    * 88f9dea Initial commit
    ```

   :sparkles:
   **Note the following:**
   * The local `main` is now pointing to the same commit as `origin/main`.
   * At this point, our local and remote repositories are once again completely
     in sync.
   * If you run `cat README.md`, you will see that the README file now contains
     the updates we made on GitHub.

   <br>

5. :owl:
   **Summary:** let's recap some of the important things we learned about
   pulling changes for a remote.
   * Synchronization between a local and a remote repo **is _not_ automatic**.
     We must trigger it `git fetch` or `git pull`.
   * **`git pull` is simply a shortcut for `git fetch` + `git merge`**. In this
     example we ran both commands one after the other (to illustrate how their
     effect differs), but you can also simply run directly `git pull`.
   * **`git fetch` always downloads data for all branches**. It can be run
     from any branch.
   * **`git pull`** also downloads data for all branches, but it
     **only updates (merge from the upstream) the currently active branch**.
     Make sure to be on the correct branch (the one to update) before running
     `git pull`.

<br>
<details><summary><b>:white_check_mark: Solution</b></summary>

```sh
# Download/retrieve new content from the remote
# (but do not update the local branch).
git fetch
git log --all --decorate --oneline --graph
git status
cat README.md

# Download/retrieve new content and update the local branch.
git pull
git log --all --decorate --oneline --graph
git status
cat README.md
```

</details>
<br>

### D) Additional Tasks: push a new branch to a remote

1. **Create a new branch named `add-more-content`** and switch to it.

2. On the new branch, **make a new commit** that adds the content below to
   the `README.md` file. Then **push the new branch** to the remote using
   **`git push -u origin add-more-content`**.

   Content for commit:

    ```md
    ## Bulleted lists
 
    **Bulleted lists** are created by adding a **`- `** or **`* `** in front
    of a line. For instance:

    `- Item 1  (or * Item 1)`  
    `- Item 2  (or * Item 2)`  
    `- ...`  
    
    will render as:

    - Item 1
    - Item 2
    - ...

    ```

   :sparkles:
   **Notes:**
   * An **upstream branch** is a remote branch that your local branch is
     linked to for **`git pull`** and **`git push`**.
     When you set an upstream branch for a local branch, Git remembers where
     the local branch should push to and pull from.
   * The **`-u`** / **`--set-upstream`** option in `git push -u origin <branch>`
     sets an **upstream branch**, linking the local branch to the remote. This
     allows future `git push` and `git pull` commands to work without
     specifying each time the remote and branch name.
   * **It is recommended** to use this option when pushing a branch for the
     first time.  
   * If you switch to a branch that **already exists on the remote** (e.g.
     created by someone else), Git will
     **automatically set the upstream branch** when you check it out. In this
     case, you don’t need to use `-u` / `--set-upstream`, even when pushing
     for the first time.  

   <br>

3. **Add another commit** to the `add-more-content` branch with the following
   content, and **push it to the remote**.

   Content for second commit:

   ```md
   ## Code blocks

   * To render text as `inline code`, surround it with single backticks **\`**.
   * To render text as a code block, use triple backticks **\`\`\`**  
   ```

   <br>

4. **Go the GitHub homepage** of your project, and verify that the content of
   the README file **on the `add-more-content` branch** has been updated.
   Be aware that, by default, GitHub shows the version of the README file from
   `main`, so you need to switch to the `add-more-content` branch in the GitHub
   interface. This is done using the **drop-down menu** near the top of the
   page.

<br>
<details><summary><b>:white_check_mark: Solution</b></summary>

```sh
# 1. Create a new branch and switch to it.
git switch -c add-more-content

# 2. Make a first commit on the new branch, and then push it to the remote.
git commit -m "Add lists to markdown guide" README.md
git push -u origin add-more-content

# 3. Make a second commit, then push to the remote again.
git commit -m "Add code blocks to markdown guide" README.md
git push
```

</details>
<br>

### E) Additional Tasks: branch cleanup

Now that our new content is ready, we can **merge it** into the `main` branch,
and then **delete the `add-new-content` branch** on our local and remote
repositories.

Perform the following tasks:

1. **Merge** `add-new-content` into `main`.
2. **Push** the changes to `main` to the remote.
3. **Delete** the branch `add-new-content` from your local repo.
4. **Delete** the branch `add-new-content` from the remote.

<br>
<details><summary><b>:white_check_mark: Solution</b></summary>

```sh
# 1. Merge 'add-new-content' into 'main'.
git switch main
git merge add-new-content

# 2. Push changes on 'main' to the remote.
git push

# 3. Delete 'add-new-content' from the local repo.
git branch -d add-new-content

# 4. Delete 'add-new-content' from the remote.
git push origin --delete add-new-content
```

</details>

<br>
<br>
<br>

## Exercise 4 - The Awesome Animal Awareness Project [60 min]

:rocket:
**Objective:** learn to collaborate with others on a project hosted on GitHub.

Congratulations! Your improving Git skills have not gone unnoticed, and you are
now hired by our agile startup to work on the
**Awesome Animal Awareness Project** !

Your mission - should you choose to accept it - is to help us build a new
website. This is a **collaborative effort**, and everyone will be working
with the same remote repository on GitHub.

Each person will contribute a web page about a specific - and awesome - animal.
At the end of the exercise, the page you contributed will be integrated
into the [Awesome Animal Awareness website (GitHub)](https://sibgit.github.io).

:fire:
**Important:**

* Please follow the **naming convention for branches** tightly,
  as our company's internal Git policies are **very strict**.
* **Before you start:** make sure that you created a
  **personal access token (PAT)** on GitHub. This will be needed in order to
  allow you to push changes to GitHub. Please refer to the course slides for
  instructions on how to create the token (a demo might also be made in the
  class before the exercise).
* To know **which animal you should work on**, please refer to the shared
  **online document** (link sent by email before the course).
* If the animal you are assigned to is **not awesome enough for you**, feel
  free to add your own animal to the list :penguin: !

<br>

### A) Clone the project repo, and create your personal work branch

1. Change into `exercise_4/`. **Clone the Awesome Animal Awareness project**,
   and **enter the cloned repo**. Here are the commands to do this:  

   ```sh
   git clone https://github.com/sibgit/sibgit.github.io
   cd sibgit.github.io
   ```

   <br>

2. **Create a new personal branch** named after your animal's name followed by
   `-dev`, and **push it to the remote** on GitHub.
   * Branch name examples: `tiger-dev`, `yeti-dev`, `sunfish-dev`,
     `pallas-cat-dev`, ...
   * :dart:
     **Hint**
     * When pushing a local branch to a remote **for the first time**,
       you have to indicate an "upstream" remote branch for the branch you are
       pushing.  
     * This is done by using the `-u / --set-upstream` option of `git push`:
        ```sh
         git push --set-upstream origin <branch you want to push>

         # Examples:
         # -u is the shortcut for --set-upstream
         git push --set-upstream origin sunfish-dev
         git push -u origin sunfish-dev
        ```

   <br>

<br>
<details><summary><b>:white_check_mark: Solution</b></summary>

```sh
# Clone and enter the project's repository.
git clone https://github.com/sibgit/sibgit.github.io
cd sibgit.github.io

# Create your personal branch and switch to it.
git switch -c yeti-dev

# Push your branch to the remote.
git push -u origin yeti-dev
```

</details>
<br>

### B) Add content for your awesome animal

You can now make edits to the webpage of your animal. For this, please make
sure that your **active branch** is your **personal branch** (and not `main`!).
If it's not the case, then **switch to your personal branch**.

Open the file corresponding to the local version of your animal's webpage in
your browser (e.g. if your animal is the manta ray, the file to open is
`manta_ray.html`). At this point, you should see that it already contains the
scaffold (structure) of the page, but it's missing content.

You task is now to populate the following fields/topics for your animal:

* Animal name
* Picture (i.e. add an image of your animal)
* Habitat and Distribution
* Diet, behavior and social organization (whatever is most relevant)
* What makes this animal awesome

**Important:** the edits for each of the above fields should be part of
a **separate commit** on your personal branch. You should thus end-up with
**5 new commits** on your personal branch.

For each of the above fields/topics, perform the following:

1. Open the the HTML file of your animal in a text editor (e.g.
   `manta_ray.html` for the Manta ray).
2. Populate the relevant field in the HTML code:
   * The **`??`** mark the positions where you have to add content (make sure
     to remove the `??` after you are done editing).
   * In the "Animal name" field, make sure to include both the common name and
     the [binomial name](https://en.wikipedia.org/wiki/Binomial_nomenclature)
     of the species, e.g. "Manta ray (_Mobula alfredi_)".
   * For the **animal picture**, you can either:
     * Link an existing image from somewhere on the web by setting
       `<img src=https://...>`.
     * Find and download a picture of your animal from the web, add the image
       to the project repo (in the `img/` directory), and insert the file name
       into the HTML file: `<img src="img/manta_ray.jpg">`.  
       **Important:** make sure to add the image file to your commit!
3. After you are done editing a field, save your changes and refresh the page
   in your web browser to see the rendered result.
4. When you are happy with the result, **create a new commit** with the
   changes.  
   :fire:
   **Important:**
   * Please give a **meaningful commit message** to your commits. E.g., if you
     added the animal name for the Manta ray, a good commit message would be:
     > Manta ray: add species common and binomial name
   * As already mentioned earlier, make sure to create a **separate commit**
     for each field/topic that you have to populate.

:dart:
**Hint:** if you want to see an example of a completed HTML page, you can
have a look at the `manta_ray.html` file.

After you populated all topics, you should have **5 new commits** on your
personal branch, which should look something like this (here exemplified
for the _manta ray_):

```txt
* 1c8aa2e manta-ray: add an awesome point about the species
* 5d8783e manta-ray: add diet and behavior info
* 9da4c8d manta-ray: add habitat and distribution info
* 068abab manta-ray: add species image
* 10035b2 manta-ray: add species common and binomial name
```

.**Push these new commits** to the remote on GitHub.

<br>
<details><summary><b>:white_check_mark: Solution</b></summary>

Here is the example of Alice, who works on the **Manta ray** page.  
First, Alice makes sure to be on her personal branch.

```sh
git switch -c manta-alice
```

Then, for each of the topics to populate, Alice performs the following tasks:

* Edit the the relevant topic in the `manta_ray.html` file using her favorite
  editor.
* When she is done, she loads/refreshes the page in her browser to make sure
  the rendering is looking good.
* Then she commits her changes.

```sh
# Make sure to be on your personal branch.
git switch -c manta-alice

# Edit the file for one of the fields/topics at a time.
# Verify the rendering of the file in your web browser.

# Stage the changes and create a new commit.
git add manta_ray.html
git commit -m "Manta ray: add species common and binomial name"
```

At the end of this process, Alice has 5 new commits on her personal branch,
that she then pushes to the remote on GitHub:

```sh
git push manta-alice
```

</details>
<br>

### D) Create a pull request / merge request

Now that your animal webpage is all populated, it's time to contribute your
work to the **`main`** branch of the project.

Since you do _not_ have the permission to push commits to the remote on the
`main` branch, you will instead contribute your changes via a **Pull Request**.
In this way, the top-level management of the Awesome Animal Awareness project
will be able to verify and approve your work before it gets added to `main`,
and becomes part of the production version of the website.

To **open a Pull Request**:

1. Go to the project's online
   [GitHub repository](https://github.com/sibgit/sibgit.github.io).
2. Under the **Pull requests** tab, click on **New pull request**.
   For more details on how to create a Pull Request, please refer to the
   course slides.
3. Once your Pull Request is merged, you should be able to see your animal's
   page rendered on the
   [Awesome Animal Awareness website (GitHub)](https://sibgit.github.io).
   **Well done!** :tada:
   * :turtle:
     It sometimes takes a few minutes before the changes become live on the
     website.

After your Pull Request was merged, you can update your local repository's
`main` branch with the newly added commits.

<br>
<details><summary><b>:white_check_mark: Solution</b></summary>

```sh
# Update the `main` branch with the changes.
git switch main
git pull
```

</details>
<br>

### E) Additional Tasks (if you have time): branch cleanup

After your team branch has been merged into the `main` branch of the project,
you can now delete your team branch and personal branches (since they have been
merged).

1. Each team member can **delete their personal branch** and **team branch**
   from their local repo.

2. One person in the team can **delete the team branch on the remote**. The
   command for this is:

    ```sh
    git push origin --delete <branch name>
    ```

   Alternatively, the team branch can also be deleted via the web interface of
   GitHub/GitLab.

3. Each team member can then **update their local copy of `main`**.

    ```sh
    git fetch --prune
    git switch main
    git pull
    ```

<br>

:pushpin:
**Note:**

* The **`--prune`** option in **`git fetch --prune`**
  **removes references to remote branches** (i.e. remote-tracking references)
  that no longer exist on the remote. For instance, if a team branch was
  deleted on the remote (after it was merged into `main`), then you will
  probably also want to delete your local reference to this remote branch from
  your local copy of the repo.  
* The `--prune` option can also be passed to `git pull`: **`git pull --prune`**.

<br>
<details><summary><b>:white_check_mark: Solution</b></summary>

```sh
# 1. Delete the remote instance of the team branch.  
#    Note: only 1 person in the team needs to do this.
git push origin --delete yeti-dev

# 2. Delete local instances of the team branch and the personal branch.
git branch -d yeti-alice
git branch -d yeti-dev

# 3. Update the local instance of the `main` branch.
#    Note the usage of the `--prune` option to delete the local pointer to
#    the former (now deleted) team branch.
git fetch --prune
git switch main
git pull
```

</details>

<br>
<br>
<br>
