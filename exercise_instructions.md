# Exercises - Introduction to Git

<br>

## Before you start

### Material download and general instructions

* **📚 Exercise material setup:** download the
  [exercises.zip](https://github.com/sib-swiss/CAS-UNIL-intro-to-git/raw/main/exercises.zip)
  archive file to your local computer and unzip it. This will unpack a
  directory named `exercises`, with all the data needed for the course
  exercises.

* **🔮 Additional Tasks:** at the end of most exercises, you will find one
  or more sections named **Additional Task**. These contain tasks to complete
  **if you have the time** and **after having completed the main exercise**.
  Additional task sections will not be corrected in class, but their solutions
  are given in this document.

* **✅ Exercise solutions:** all exercises and Additional Task sections have
  their solutions embedded in this document. Solutions are hidden by default,
  but you can reveal them by clicking on them. Here is an example:

  <details><summary><b>Exercise solution (click to reveal)</b></summary>
  ✨ This reveals the answer ✨
  </details>

  We encourage you to **not look at the solutions too quickly**, and try to
  solve the exercises without them. Remember that you can always ask the
  course teachers for help.

* **🔥 Tip:** if you are viewing these instructions on the GitHub web interface,
  you can display a table of contents (outline) by clicking on the small icon
  that looks like a bulleted list near the top-right of this page.

<br>

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

To see the config values as currently set, you can use:

```sh
git config --get user.name
git config --get user.email
git config --get core.editor

# Show all config values at once and where they are set.
git config --list --show-origin
```

<br>
<br>
<br>

## Exercise 1 - Your first commit [~30 min]

**🚩 Objective:** learn to create a new Git repository, add files to it, and
make commits.

<br>

Welcome to the first exercise of this Git course! This is a warm-up, so you
will be guided step-by-step on exactly what you need to do.

### Part A - Create a new repo from scratch and make a first commit

1. **Change directory** into `exercise_1/test-project` and list the
   directory's content using the following shell commands:

   ```sh
   cd exercise_1/test-project   # Enter the directory.
   ls -l                        # List files present in the directory.
   ```

   You should see that it contains files reminiscent of a simple scripting
   project, e.g. a data analysis pipeline (here written in Python).

   Here is a depiction of the directory's structure in more detail than
   the output of `ls -l`.

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
   directory (i.e. turn `test-project` into a Git repo):

   ```sh
   git init   # Initialize a new Git repository.
   ```

   Initializing a new Git repo creates a **hidden `.git` directory**. You
   can view this directory by running the shell command:

   ```sh
   ls -la   # You should observe that a new ".git" hidden directory was created.
   ```

   <br>

   > **🔥 Important:** the **`.git`** directory is where Git stores the entire
   > history of your repository (as well as various settings). If you delete
   > it, all your version control history (and settings) for this repository
   > **will be lost**. You can do this if e.g. you want to start the exercise
   > from scratch again.

   > **✨ Note:** creating a new repo locally with `git init` is arguably _not_
   > the most frequent way of starting a new repository. Instead, people will
   > often first create a project on a Git hosting service
   > ([GitHub](https://github.com/), [GitLab](https://gitlab.com),
   > [Codeberg](https://codeberg.org)), and then clone their new repo locally
   > to start working on it.

   <br>

3. **Display the status of files** in the working tree (i.e. the `test-project`
   directory) by running the command:

    ```sh
    git status
    ```

   **❓ Question:** what is the status of the files in your working directory ?

    <details><summary><b>✅ Solution</b></summary>
    <br>

    Running `git status`, we see - as expected at this point - that all files
    are **untracked**.

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
   files - these are Python cache files we don't want to track).

   > **🦉 Reminder:** _staging_ a file is a synonym of
   > _adding to the Git index_. The command to stage files is: `git add`.
   > For instance, `git add README.md` will stage the file `README.md`.

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

   <details><summary><b>✅ Solution</b></summary>
   <br>

   There are several ways we can stage all the requested files:

   * By staging individual files:

     ```sh
      git add README.md
      git add script.py
      git add doc/
      git add tests/tests.py
      git add tests/output.csv

      # Note that we can also stage all files in a single command:
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

     Note that in this specific case we cannot use `git restore --staged` to
     remove files from the Git index because we do not have any commits yet
     in the repository (and `git restore --staged` needs a commit to restore
     from).

   If we now run `git status`, we should see that all our files except the
   `.pyc` files are displayed as "new file" under "Changes to be committed:",
   and are ready to be committed.

   </details>
   <br>

5. **Add a first commit** to the repo with the commit message
   `Initial commit for test project`. The command is the following:

     ```sh
     git commit -m "Initial commit for test project"

     # Same as above, but with the "long form" of the -m option.
     git commit --message "Initial commit for test project"
     ```

6. **Display the repository's history** with `git log`.

    ```sh
    git log
    ```

   At this point you should have a single commit that looks like the following
   (your exact values for commit hash, date, etc. will differ):

    ```txt
    commit a0da6303e9d6dfc34986f959a076721e153f382d (HEAD -> main)
    Author: Your Name <your.email@example.org>
    Date:   Mon Oct 14 13:55:08 2024 +0200

        Initial commit for test project
    ```

   Let's now have a look at the content of our new commit:

    ```sh
    git show
    ```

   > **✨ Note:** when the amount of text printed by `git show` exceeds
   > one screen, the content is shown with the GNU program `less`. In
   > `less`, you can use your keyboard arrow keys to move up/down, and
   > press `q` to exit and return to the shell.

   **❓ Question:** why are the details of the `doc/user-guide.pdf` file not
   displayed by `git show` ?

    <details><summary><b>✅ Solution</b></summary>
    <br>

    Looking at the output of **`git show`**, we can see that the newly added
    content for the file `doc/user-guide.pdf` is not displayed - unlike for
    `README.md` where the content of the file is shown.

    The reason is that `user-guide.pdf` is a **binary file** and not a
    plain text file. Git does not display details for binary files.

    </details>
    <br>

### Part B - Commit a change to a tracked file

In this section, we will make an update to the `README.md` file, and then
create a new commit that adds the change we made.

1. **Open the `README.md` file** in your favorite text editor.
   * Change the 3rd line of the file to:
     > Demo project for the Git course. This will be great!
   * Save your changes and close the file.
   * Run **`git status`**. The `README.md` file should now be listed as
     modified:

      ```txt
      Changes not staged for commit:
          modified:   README.md

      Untracked files:
          script.pyc
          tests/tests.pyc
      ```

   <br>

2. **Display the changes** to files in the working tree using the command
   **`git diff`**, which displays the difference in tracked files between the
   working tree and the Git index (staging area).

    ```sh
    git diff
    git diff README.md  # Gives the same result, as only README.md was modified.
    ```

   You should see that `README.md` has one line removed (shown in red, prefixed
   with `-`), and one line added (shown in green, prefixed with `+`).

    ```diff
    -Demo project for the Git course.
    +Demo project for the Git course. This will be great!
    ```

   <br>

3. **Commit the changes** you just made:

   * Add/stage the changes made to `README.md` with **`git add`**.
     Remember that each time you modify a file and want to include these
     changes into your next commit, you have to `git add` that file again.

     > **🔥 Tip:** to stage all modified files at once, you can use the shortcut
     > **`git add -u`**. Here it does not make a lot of difference as there is
     > only 1 modified file, but if there are many of them, this command is a
     > useful shortcut.

   * Run `git status` again: you should see that the `README.md` file is now
     listed under `Changes to be committed:` (in green).

   * Commit your changes with the message `"Make README file more cheerful"`.

    <details><summary><b>✅ Solution</b></summary>
    <br>

      ```sh
      git add README.md
      git commit -m "Make README file more cheerful"

      # Alternative: stage all modified files with "git add -u". Since only
      # README.md was modified, this is the same as staging README.md.
      git add -u
      git commit -m "Make README file more cheerful"

      # Alternative: use a "git commit" shortcut to stage + commit in a single
      # command.
      git commit -m "Make README file more cheerful" README.md
      git commit --all -m "Make README file more cheerful"
      ```

    </details>
    <br>

### Part C - Add files to the `.gitignore` list

At this point, the only files that should be left **untracked** in our
repository are the two `*.pyc` files (you can verify this by running
`git status`). Since we are never going to track these files, we would like
to **permanently ignore** them, so that they stop being listed as _untracked_.

1. At the root of the working tree, **create a text file named `.gitignore`**,
   with the following content:

    ```txt
    *.pyc
    ```

   > **🔥 Tip:** you can create the `.gitignore` in any text editor you like,
   > but you can also easily generate it with a shell command:
   >
   > ```sh
   > echo "*.pyc" > .gitignore
   > ```

    <br>

2. **Run `git status`**: you should see that you still have an untracked
   file: the `.gitignore` file you just created 😅 !

   Since the ignore rules defined in the `.gitignore` file are useful to all
   users of the repository, this file should be added to the repo.

   * Stage the `.gitignore` file.
   * Make a new commit with the commit message `Add *.pyc to gitignore list`.
     At the end of this step, your working tree should now be clean: when you
     run `git status`, the output should be:

      ```txt
      On branch main
      nothing to commit, working tree clean
      ```

   <details><summary><b>✅ Solution</b></summary>
   <br>

     ```sh
     # Stage the .gitignore file.
     git add .gitignore

     # Make a new commit.
     git commit -m "Add *.pyc to gitignore list"

     # The working tree is now clean.
     git status  # -> nothing to commit, working tree clean
     ```

   </details>
   <br>

3. **Display the (modest) history of your Git repo** with the following
   variations of the **`git log`** command. Observe how history is displayed
   by each command:

    ```sh
    git log                   # Prints the full commit message along with author and date.
    git log --pretty=oneline  # One commit per line. Full commit hash/ID (checksum).
    git log --oneline         # One commit per line. Abbreviated commit hash/ID.
    git log --all --decorate --oneline --graph  # Shows commits for all branches.
    ```

    With the current history of our Git repo, the output of
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
      * 81d03aa (HEAD -> main) Add *.pyc to gitignore list
      * 029a389 Make README file more cheerful
      * da59f94 Initial commit for test project
      ```

    > **🔥 Tip:** to list your Git aliases: `git config --list | grep ^alias`

<br>

### 🔮 Additional task - Remove content from the Git index (unstaging)

**🔨 Setup:** for this task, we will need an additional file named
`personal_notes.md`, as well as a change in the `script.py` file. Let's
generate this file/changes by running the following commands at the
root of the `test-project` directory:

```sh
echo "Let's keep this local" > personal_notes.md
echo "adding a bad line..." >> script.py

# You can then visualize the changes by running:
git status
git diff
```

We are now ready to start our tasks for this section. Start by
**staging all untracked and modified files** with:

```sh
git add --all
```

The status of your files should then look like:

```txt
Changes to be committed:
    new file:   personal_notes.md
    modified:   script.py
```

Actually, we do _not_ want to add these changes to the repository,
so **let's unstage them**.

```sh
# Unstage the changes to script.py.
git restore --staged script.py
```

```sh
# Unstage the entire personal_notes.md file.
git restore --staged personal_notes.md

# Alternatively, in the case of a newly added file, we can also use.
git rm --cached personal_notes.md
```

> **🦉 Reminder:** the difference between `git rm --cached` and
> `git restore --staged` is that `git rm --cached` removes the entire
> file from the index, while `git restore --staged` reverts it to the
> version in the last commit (`HEAD` commit).

> **✨ Note:** the reason why in this particular case `git rm --cached` does
> exactly the same as `git restore --staged` is because `personal_notes.md`
> is a newly added file. There is thus no difference between removing it
> completely, or just resetting it back to its version from the latest
> commit (since it is absent from the latest commit).

> **⚠️ Warning:** be careful to _not_ run `git rm` instead of
> `git rm --cached`, as this would not only remove the file from the
> Git index, but also delete it from your working tree!

At this point, changes in `script.py` should again be unstaged, and
`personal_notes.md` should be untracked. Run **`git status`** to confirm this:

```txt
Changes not staged for commit:
  modified:   script.py

Untracked files:
  personal_notes.md
```

<br>

### 🔮 Additional task - Staging shortcuts: `--update` vs. `--all`

> **🦉 Reminder:**
>
> * **`git add --all`:** updates the Git index with all modified and untracked
>   files.
> * **`git add --update`:** updates the Git index only with the new versions
>   of files that are already tracked. It does _not_ stage any new, untracked
>   files. In a sense, `--update`/`-u` is safer because it prevents you from
>   adding completely new files to the Git repo by mistake.

In the task just above, we have used `git add --all` to stage all modified and
untracked files present in our repo. Now we would like to stage only the
modified file `script.py`.

* Run the command **`git add -u`**, then look at the status of your files.
  You should see that only `script.py` was staged (because it's a modified
  file), but not `personal_notes.md` (because it's untracked).

  ```sh
  git add -u   # -u is the shortcut for --update.
  git status
  ```

  ```txt
  Changes to be committed:
    modified:   script.py

  Untracked files:
    personal_notes.md
  ```

* Run `git restore --staged script.py` to unstage the changes to `script.py`.

<details><summary><b>✅ Solution</b></summary>
<br>

The difference between `git add --update` and `git add --all` is that
**`--update`** only adds files that are already tracked in Git, while
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

### 🔮 Additional task - Ignoring files with `.git/info/exclude`

`personal_notes.md` is a file that we never intend to track and share with
other people. Therefore we would like to **ignore** it. However, since this
file is specific to our own local setup, it should only be ignored by our
local Git repo, and not by everyone else.

> **🦉 Reminder:** in Git, files/patterns to ignore only in your local repo
> should be added to **`.git/info/exclude`**. This is a text file that is
> automatically present in a `.git` repo.

* Edit the file **`.git/info/exclude`** to ignore `personal_notes.md`.
  You can do this with a regular text editor, or using the following
  shell command:

    ```sh
      echo "personal_notes.md" >> .git/info/exclude

      # Display the content of the file:
      cat .git/info/exclude
    ```

* Run `git status` again. The file `personal_notes.md` should no longer
  be listed as _untracked_.

<details><summary><b>✅ Solution</b></summary>
<br>

```sh
  # Add 'personal_notes.md' to the "exclude" file:
  echo "personal_notes.md" >> .git/info/exclude
  cat .git/info/exclude  # Display the content of the file.

  # 'personal_notes.md' is no longer listed as untracked.
  git status
```

</details>
<br>

### 🔮 Additional task - Resetting changes with `git restore`

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
* Run `cat script.py` to make sure the "bad line" has been removed from the
  file.
* Run `git diff`: there should be no difference anymore (no output).
* Run `git status`: at this point, your working tree should be clean.

  ```txt
  On branch main
  nothing to commit, working tree clean
  ```

<br>

> **⚠️ Warning:** as you have just experienced, `git restore <file>` really
> overwrites uncommitted modifications in your files. Use this command
> carefully to avoid losing work by mistake.

<details><summary><b>✅ Solution</b></summary>
<br>

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

### 🔮 Additional task - Remove a file from a repository with `git rm`

Currently the file `tests/output.csv` is being tracked in our Git repo.
However, all things considered, this file is not really needed, and we now
would like to delete it from both our repo and working tree.

* Delete the file with **`git rm`**.
* Run `git status`: you should see that the file was deleted, and that this
  deletion is already staged.

  ```txt
  On branch main
  Changes to be committed:
    deleted:    tests/output.csv
  ```

* Make a new commit that removes this file from the repo. You can use the
  commit message `Remove test output`.

> **🦉 Reminder:** even though we are deleting `output.csv`, a copy of it
> will remain in the history of our repository.

<details><summary><b>✅ Solution</b></summary>
<br>

```sh
git rm tests/output.csv
git status
git commit -m "Remove test output"
```

We use **`git rm`** to remove `tests/output.csv` from both the Git index
and the working tree. To delete the file only from the index we would use:

```sh
git rm --cached tests/output.csv
```

</details>
<br>

### 🔮 Additional task - Retrieve files with `git restore --source`

Let's imagine that, for some reason, we want to retrieve the file
`tests/output.csv` from our commit history, specifically from our
second-to-last commit (i.e. the commit before the last).

Try to do so using the following command. You need to replace `<commit ref>`
with the commit ID (or another reference to the commit) of the commit from
which to retrieve the file.

```sh
git restore --source <commit ref> tests/output.csv
```

> **🔥 Tip:** you can use `HEAD~1` to refer to the second-to-last commit.

<details><summary><b>✅ Solution</b></summary>
<br>

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

## Exercise 2 - The Git reference web page [~30 min]

**🚩 Objective:** learn to use a basic branched workflow.

<br>

Well done! Your burgeoning Git skills have landed you a job as a junior
web developer at _Scamazone Inc._, where you have been assigned the gratifying
task of fixing bugs in their website.

Let's get started:

1. Change directory into `exercise_2/`. You will see that it already contains
   a Git repository, as well as an HTML page named `references.html`.
2. Open the `references.html` page in your web browser.
3. Explore the content of the Git repo using the `git log`, `git status` and
   `git branch` commands.

**❓ Questions:**

* How many commits have already been made in the repo ?
* How many branches are present in the repo ? How are they named ?
* Are there any uncommitted changes ?

<details><summary><b>✅ Solution</b></summary>
<br>

```sh
cd exercise_2/
git log
git log --oneline  # There are 3 commits in the repo.
git branch         # There is currently only 1 branch: main.
git status         # There is one tracked file with uncommitted changes: references.html
```

</details>
<br>

### Part A - Fix the broken "ProGit" link

Your first task is to fix the broken link to the "ProGit" book in the HTML
page (currently when you click on the "ProGit" link with the webpage open in
your browser, it returns an error).

Since **we want to follow good practices**, we will _not_ work on this fix in
the **`main` branch**. Instead we will create a new branch, fix the link
problem on that branch, test our fix, and then merge it into `main` once we
are confident the problem is solved.

The reason we proceed in this way is that `main` is the branch that is used
to generate the **production version** of the Scamazone website (the version
that customers can see), and we don't want to make changes to that production
version until we are really sure that the changes we introduce in the code are
working as expected.

1. Before working on our fix in a new branch, we need to
   **make sure that our working tree is clean**:
    * Check the Git repo to see if there are any uncommitted changes.
    * If there are, display the uncommitted changes to see what they do. Even
      if you are not familiar with HTML, it should be easy enough to figure out
      what the changes do.

2. Now that you have figured out what the uncommitted changes do, stage the
   changes and **make a commit with a meaningful message**. Verify that your
   working tree is now clean.

3. **Create a new branch named `fix`** and switch to it. This `fix` branch is
   where we will work on our bug fix, so that our changes to the code base
   remain isolated from the `main` branch until we are sure our fix is fine.

4. On the new branch, edit the `references.html` file to fix the link to the
   "ProGit" book.

   > **🎯 Hint:** to fix the link, add `https://` in front of the URL.

5. **Verify in your browser** that the link is now working properly (you might
   have to reload the page). You can then commit your changes.
   Please **use a meaningful commit message**.

6. Now that our bug fix is production ready,
   **merge the `fix` branch into `main`**.

   Then, display the history of your repo again:

     ```sh
     # You can also use 'git adog' if you have created this alias.
     git log --all --decorate --oneline --graph
     ```

   At this point, the output should look like this
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

<details><summary><b>✅ Solution</b></summary>
<br>

1. Check if the working tree is clean, and see uncommitted changes.

   ```sh
    git status   # This shows that the references.html file has uncommitted changes.
    git diff     # Display the uncommitted changes in the file.
   ```

2. Commit the changes.

    ```sh
     git add references.html  # Stage the changes in the references.html file.
     git commit -m "Add Git logo placeholder to the Git reference webpage"

     # You can also use these shortcuts for the above 2 lines:
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

### 🔮 Additional task - Add an image and new links

To further improve our Git reference web page, you are now tasked with adding
a couple of new book links to the page.

1. Work in a **new branch** named `dev`.

2. **Make a commit** that adds the following 2 references at the end of the
   list in the HTML page:

   ```html
    <li><a href="https://www.manning.com/books/learn-git-in-a-month-of-lunches">
      Learn git in a month of lunches
    </a></li>
    <li><a href="https://www.amazon.com/Git-Porch-Willie-Crawford-2006-02-01/dp/B01K95YGYG">
      Git Porch
    </a></li>
   ```

    Use **`git diff`** and **`git diff --cached`** to look at your edits in
    the HTML file, before and after staging them.

    **❓ Question:** what is the difference between `git diff` and
    `git diff --cached` ?

    Check whether you did a proper job by refreshing the HTML page in your
    browser _before_ you commit your changes.

   <br>

3. **Make a commit** that adds the Git logo to the webpage.

   Replace the placeholder line that starts with
   `<!-- Add Git logo placeholder` by `<img src="git_logo.png">` in the
   HTML file.

   > **✨ Note:** check whether you did a proper job by refreshing the HTML
   > page in your browser _before_ you commit your changes.

4. When you have added these new features - and tested that they actually work
   by reloading the `references.html` page in your browser (new links are
   working, logo was added) - merge your `dev` branch into `main`.

5. **Display your repository's history** with the command:

    ```sh
    git log --all --decorate --oneline --graph
    ```

   It should look like this (commit ID values will differ and commit messages
   may be different):

   ```txt
    * ba4687a (HEAD -> main, dev) Add Git logo
    * 30b149a Add two new books to Git reference page
    * 50a2e7f Fix broken ProGit link
    * e6a6176 Add Git logo placeholder to the Git reference webpage
    * 8a7444c Add Git logo file
    * c99fb57 Add links to Git resources
    * 5b54605 First commit. Add template for the Git reference page
   ```

6. **Delete the `dev` branch**, you no longer need it. Verify it was deleted
   by running:

    ```sh
     git branch
     # and / or:
     git log --all --decorate --oneline --graph
    ```

<details><summary><b>✅ Solution</b></summary>
<br>

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

**❓ Question answer:** the difference between `git diff` and `git diff --cached`
is that the former will display the difference between the working tree
(files on disk) and the Git index, while the latter shows the difference
between the Git index and the latest commit.

Add a Git logo to web page:

```sh
# 3. Edit the HTML page to add logo.
vim references.html
git commit -m "Add Git logo" references.html
```

Merge changes into `main`, delete branch `dev`:

```sh
# 4. Merge "dev" into "main".
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

## Exercise 3 - The markdown cheat-sheet [~30 min]

**🚩 Objectives:**

* Create a repo on GitHub.
* Practice the basic commands to interact with a remote: `git push`,
  `git pull`, and `git fetch`.

<br>

Looking good so far! It's now time we add a new moving piece: working
with **remote repositories**.

In this exercise, we will work on a small - and incomplete - cheat-sheet for
the **[Markdown language](https://www.markdownguide.org) syntax**. As you
probably sensed, this is a project of prime importance, and therefore we will
want to set up a **remote repository** for the project on GitHub, so that we
can:

* Have a backup of our work on GitHub.
* Make it available to everyone out there.

> **🔥 Important:**
>
> * **Before you start** this exercise, make sure you created a
>   **personal access token (PAT)** on GitHub. This will be needed to push
>   commits to your repo on GitHub. A demo on how to create a PAT will be made
>   in the class (if this has not been done yet, please kindly remind the
>   teacher to do so 😇 - thank you).
>
>   If you are doing the exercise on your own, instructions on how to create
>   a PAT can also be found in the course slides.

<br>

### Part A - Create a new repo on GitHub

1. In your web browser, connect to your GitHub account and
   **create a new repository** with the following characteristics:
   * **Repository name:** `test-markdown-guide`
   * **Repository description:**
     `Test repository to learn working with remotes`
   * Visibility: **public** (anyone has read access).
   * **Add README** switch: on.

   <br>

   > **✨ Note:** if needed, you can find instructions on how to create a new
   > repository in the course slides.

2. On your computer, enter the directory `exercise_3/` and
   **clone your new repository**.

    ```sh
     # Note: replace <user name> with your GitHub user name.
     git clone https://github.com/<user name>/test-markdown-guide.git
    ```

   This command **`git clone`** downloads a copy of the entire remote repo
   to your computer, and also sets the repo of GitHub as the **remote** of
   your local repo. By default, this remote is named **`origin`** - this is
   why e.g. the pointer to the `main` branch on the remote is named
   `origin/main`.

3. **Enter the directory** you just cloned and list its content:

    ```sh
     cd test-markdown-guide
     ls -l
    ```

   You should see that all it contains is a `README.md` file. This file was
   automatically added by GitHub because we asked it to do so when configuring
   the new repo.

4. **Display the content** of the README file with the command:

   ```sh
   cat README.md
   ```

<br>

### Part B - Push commits to the remote

Let's now modify the content of the `README.md` file and push those changes
to the remote.

1. **Change the content of `README.md`** to the following text (you can copy
   the text using the icon on the top-right of the text block).

    ```md
    # A short primer on markdown syntax ! 💫

    Markdown is a lightweight markup language that you can use to add
    formatting to plaintext text documents. Markdown is one of the most
    popular markup languages.

    Markdown files with a `.md` extension - such as this README file -
    are automatically rendered by Git hosting services (e.g. GitHub or GitLab).
    ```

2. **Run `git diff` and `git status`** to display changes you made to
   `README.md`.

3. **Make a new commit** with your changes (use a meaningful commit message).

   <details><summary><b>✅ Solution</b></summary>
    <br>

    ```sh
     # Stage the README fine, then create a new commit.
     git add README.md
     git commit -m "Update markdown guide title"
     
     # Alternatively, we could also stage and commit in a single command.
     git commit -m "Update markdown guide title" README.md
    ```

    </details>

4. **Display the history** of your repo with the command (or use
   the `git adog` alias, if you created it):

    ```sh
    git log --all --decorate --oneline --graph
    ```

    You should have 2 commits (commit ID values will differ):

    ```txt
    * 65efd2c (HEAD -> main) Update markdown guide title
    * 88f9dea (origin/main, origin/HEAD) Initial commit
    ```

   What you should pay attention to, is the respective positions of the
   **`main`** and **`origin/main`** branches:

   * **`main`** (in green) shows the position of the `main` branch in
     our **local repo**. It is pointing to the 2nd commit, the one we just
     added (`65efd2c` in the example above).
   * **`origin/main`** (in red) shows the position of the `main` branch on
     the **remote**. Currently, `origin/main` is still pointing to the initial
     commit (`88f9dea`), because we have not pushed our new changes (commits)
     to the remote.

   <br>

   > **✨ Note:** to be completely accurate, we should say that `origin/main`
   > shows the **last known position of the `main` branch** on the remote.
   > Remember that synchronization between a local and remote repo is _not_
   > automatic.

   <br>

   In this situation, the local `main` branch is said to be **ahead** of the
   remote: the new commit we just made (`65efd2c`) is only present on our
   local computer. If we lost access to our computer just now (e.g. it gets
   stolen while we enjoy one too many beers at the bar - true story), we would
   permanently lose the work we did in that last commit.

   > **🔥 Tip:** running the command:
   >
   > ```sh
   > git status
   > ```
   >
   > also warns us about the discrepancy between `main` and `origin/main` -
   > see the 2nd line of the output below:
   >
   > ```txt
   > On branch main
   > Your branch is ahead of 'origin/main' by 1 commit.
   >   (use "git push" to publish your local commits)
   >
   > nothing to commit, working tree clean
   > ```

    <br>

5. **Push your changes on `main` to the remote** with the command:

    ```sh
    git push
    ```

   Then display the history of your repo again. You should now see that both
   `main` and `origin/main` point to the latest commit.

    ```txt
    * 65efd2c (HEAD -> main, origin/main, origin/HEAD) Update markdown guide title
    * 88f9dea Initial commit
    ```

    Likewise, running **`git status`** now tells us that our local `main` is
    up to date with the remote (2nd line of the output below).

    ```txt
    On branch main
    Your branch is up to date with 'origin/main'.

    nothing to commit, working tree clean
    ```

    <br>

6. At this point, our **local and remote repositories are perfectly in sync**:
   all commits that we have locally are also present on the remote and vice
   versa.

   To convince yourself that this is indeed the case, go to your project home
   page on GitHub: you will see that the updated version of the `README.md`
   file is displayed.

<br>

### Part C - Pull changes from the remote

In this exercise you are working on your project alone - no one else is pushing
changes to your remote. To simulate content being added to the remote, we will
therefore use a small trick: you will add a commit to your repo via the
GitHub **web interface**.

1. **Go to the home page** of your project on GitHub and click on the
   **Edit file** button (the small pencil icon displayed at the top-right
   of the `README.md` file - make sure you are signed in).

2. The `README.md` file is now in edit mode:
   **copy-paste the following content** at the end of the file - do not remove
   what is already in the file, just add to it:

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

   Then click on the **"Commit changes..."** button to create a new commit on
   the `main` branch with the commit message:

   > Add bold, italic and titles to markdown guide

   After the changes are committed, you will see that the README file on
   your project's home page on GitHub gets updated with the new content.

   <br>

3. **Sync the local and remote repos.**

   At this point, the new changes are **only present on the remote**. To
   convince yourself, you can try to run:

   ```sh
   cat README.md
   ```

   You will see that your local README's content has not been updated (because
   Git does not automatically sync a local and remote repo - it's a manual
   operation).

   Let's now **sync our local repo** with the new content from the remote. Run
   the command:

   ```sh
   git fetch
   ```

   This retrieves (downloads) all new content from the remote to our local
   repo. However, it will _not_ update the local `main` branch. To verify
   this, display your repo history, which should look like this:

    ```txt
    * efe768e (origin/main, origin/HEAD) Add bold, italic and titles to markdown guide
    * 65efd2c (HEAD -> main) Update markdown guide title
    * 88f9dea Initial commit
    ```

   > **✨ Note:**
   >
   > The new commit we made on the remote (`efe768e` in the example above)
   > has been downloaded to our local repo: the data is now present on our
   > computer and our history has 3 commits.
   >
   > However, our local `main` branch is still pointing at the 2nd commit
   > (`65efd2c`) - the new commit was _not_ merged into the local `main`.
   >
   > You can try to run the command `cat README.md` to display the content of
   > the README file: you will see that it does _not_ yet contain the new text
   > that we added on GitHub.

   <br>

4. **Let's update our local `main` branch**: run the command: **`git pull`**,
   then display your repo history again:

    ```txt
    * efe768e (HEAD -> main, origin/main, origin/HEAD) Add bold, italic and titles to markdown guide
    * 65efd2c Update markdown guide title
    * 88f9dea Initial commit
    ```

   As you can see, the local `main` is now pointing to the same commit as
   `origin/main`. At this point, our local and remote repositories are
   completely in sync.

   If you run `cat README.md` again, you will see that the README file now
   contains the updates we made via the GitHub web interface.

   <br>

**💫 Summary:** let's recap some of the important things we learned about
pulling changes from a remote.

* Synchronization between a local and a remote repo is _not_ automatic.
  We must trigger it with `git fetch` or `git pull`.
* **`git pull`** is simply a shortcut for `git fetch` + `git merge`. In this
  example we ran both commands one after the other (to illustrate how their
  effect differs), but you can also simply run directly `git pull`.
* **`git fetch`** always downloads data for all branches. It can be run
  from any branch.
* **`git pull`** also downloads data for all branches, but it
  only updates (merges from the upstream) the currently active branch.
  Make sure to be on the correct branch (the one to update) before running
  `git pull`.

<details><summary><b>✅ Solution</b></summary>
<br>

```sh
# Download/retrieve new content from the remote (does not update the local branch).
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

### 🔮 Additional task - Push a new branch to a remote

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

   > **✨ Notes:**
   > * An **upstream branch** is a remote branch that your local branch is
   >   linked to for **`git pull`** and **`git push`**.
   >   When you set an upstream branch for a local branch, Git remembers where
   >   the local branch should push to and pull from.
   > * The `-u`/`--set-upstream` option in
   >   `git push -u origin <branch>` sets an **upstream branch**, linking
   >   the local branch to the remote. This allows future `git push` and
   >   `git pull` commands to work without specifying each time the remote
   >   and branch name.
   > * **It is recommended** to use this option when pushing a branch for the
   >   first time.
   > * If you switch to a branch that **already exists on the remote** (e.g.
   >   created by someone else), Git will
   >   **automatically set the upstream branch** when you check it out. In this
   >   case, you don't need to use `-u`/`--set-upstream`, even when pushing
   >   for the first time.

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

4. **Go to the GitHub homepage** of your project, and verify that the content
   of the README file **on the `add-more-content` branch** has been updated.
   Be aware that, by default, GitHub shows the version of the README file from
   `main`, so you need to switch to the `add-more-content` branch in the GitHub
   interface. This is done using the **drop-down menu** near the top of the
   page.

<details><summary><b>✅ Solution</b></summary>
<br>

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

### 🔮 Additional task - Branch cleanup

Now that our new content is ready, we can **merge it** into the `main` branch,
and then **delete the `add-more-content` branch** on our local and remote
repositories.

Perform the following tasks:

1. **Merge** `add-more-content` into `main`.
2. **Push** the changes to `main` to the remote.
3. **Delete** the branch `add-more-content` from your local repo.
4. **Delete** the branch `add-more-content` from the remote.

<details><summary><b>✅ Solution</b></summary>
<br>

```sh
# 1. Merge 'add-more-content' into 'main'.
git switch main
git merge add-more-content

# 2. Push changes on 'main' to the remote.
git push

# 3. Delete 'add-more-content' from the local repo.
git branch -d add-more-content

# 4. Delete 'add-more-content' from the remote.
git push origin --delete add-more-content
```

</details>
<br>
<br>
<br>

## Exercise 4 - The Awesome Animal Awareness Project [~60 min]

**🚩 Objective:** learn to collaborate with others on a project hosted on
GitHub.

<br>

Congratulations! Your newly-acquired Git skills have not gone unnoticed - and
you have now been hired by our agile startup to work on the
**Awesome Animal Awareness Project**!

Your mission - should you choose to accept it - is to help us build a new
website. This is a **collaborative effort**, and everyone will be working
with the same remote repository on GitHub.

Each person will contribute a web page about a specific - and awesome - animal.
At the end of the exercise, the page you contributed will be integrated
into the [Awesome Animal Awareness website (GitHub)](https://sibgit.github.io).

> **🔥 Important:**
>
> * To know **which animal you should work on**, please refer to the shared
>   **online document** (link sent by email before the course).
> * Should the animal you are assigned to **not be awesome enough for you**,
>   feel free to add your own animal to the list 🦩!
> * If not already done earlier, please create a GitHub
>   **personal access token (PAT)**. This will be needed to push commits to
>   GitHub. A demo on how to create a PAT will be made in the class (if this
>   has not been done yet, please kindly remind the teacher to do so 😇 -
>   thank you). If you are doing the exercise on your own, instructions on how
>   to create a PAT can also be found in the course slides.

<br>

### Part A - Clone the project repo and create your personal work branch

1. Change into `exercise_4/`. **Clone the Awesome Animal Awareness project**,
   and **enter the cloned repo**. Here are the commands to do this:

   ```sh
   git clone https://github.com/sibgit/sibgit.github.io
   cd sibgit.github.io
   ```

   <br>

2. **Create a new personal branch** named after your animal's name followed by
   `-dev`, and **push it to the remote** on GitHub.

   Branch name examples: `tiger-dev`, `yeti-dev`, `sunfish-dev`,
   `pallas-cat-dev`, ...

   > **🎯 Hint:**
   >
   > When pushing a local branch to a remote **for the first time**, you have
   > to indicate an "upstream" remote branch for the branch you are pushing.
   >
   > This is done by using the `-u`/`--set-upstream` option of `git push`:
   >
   > ```sh
   >  git push --set-upstream origin <branch you want to push>
   >
   >  # Examples:
   >  # -u is the shortcut for --set-upstream
   >  git push --set-upstream origin sunfish-dev
   >  git push -u origin sunfish-dev
   > ```

<details><summary><b>✅ Solution</b></summary>
<br>

The solution is here exemplified with the **manta ray**. Simply replace `manta`
with your animal name.

```sh
# Clone and enter the project's repository.
git clone https://github.com/sibgit/sibgit.github.io
cd sibgit.github.io

# Create your personal branch and switch to it.
git switch -c manta-dev

# Push your branch to the remote.
git push -u origin manta-dev
```

</details>
<br>

### Part B - Add content for your awesome animal

You can now make edits to the webpage of your animal. For this, please make
sure that the **active branch** is your **personal branch** (and not
**`main`**!). If it's not the case, then **switch to your personal branch**.

Open the file corresponding to the local version of your animal's page in
your browser (e.g. if your animal is the manta ray, the file to open is
`manta_ray.html`). At this point, you should see that it already contains the
scaffold (structure) of the page, but it's missing content.

Your task is now to populate the following fields/topics for your animal:

* Animal name
* Picture (i.e. add an image of your animal)
* Habitat and distribution
* Diet, behavior and social organization (whatever is most relevant)
* What makes this animal awesome

> **🔥 Important:**
>
> The edits for each of the above fields should be part of a separate commit
> on your personal branch. You should thus end up with **5 new commits** on
> your personal branch.

For each of the above fields/topics, perform the following:

1. Open the HTML file of your animal in a text editor (e.g.
   `manta_ray.html` for the Manta ray).

2. Populate the relevant field in the HTML code: the **`??`** mark the
   positions where you have to add content (make sure to remove the `??` after
   you are done editing).

   In the "Animal name" field, make sure to include both the common name and
   the [binomial name](https://en.wikipedia.org/wiki/Binomial_nomenclature)
   of the species, e.g. "Manta ray (_Mobula alfredi_)".

   For the **animal picture**, you can either:
   * Link an existing image from somewhere on the web by setting
     `<img src=https://...>`.
   * Find and download a picture of your animal from the web, add the image
     to the project repo (in the `img/` directory), and insert the file name
     into the HTML file: `<img src="img/manta_ray.jpg">`.

     > **🔥 Important:** make sure to add the image file to your commit!

3. After you are done editing a field, **save your changes and refresh** the
   page in your web browser to see the rendered result.

4. When you are happy with the result, **create a new commit** with the
   changes.
   * Please give a **meaningful commit message** to your commits. If you
     added the animal name for the Manta ray, a good commit message would be:

     > Manta ray: add species common and binomial name

   * As already mentioned earlier, make sure to create a **separate commit**
     for each field/topic that you have to populate.

<br>

> **🎯 Hint:** if you want to see an example of a completed HTML page, you can
> have a look at the `manta_ray.html` file.

<br>

After you populated all topics, you should have **5 new commits** on your
personal branch, which should look something like this:

```txt
* 1c8aa2e manta-ray: add an awesome point about the species
* 5d8783e manta-ray: add diet and behavior info
* 9da4c8d manta-ray: add habitat and distribution info
* 068abab manta-ray: add species image
* 10035b2 manta-ray: add species common and binomial name
```

**Push these new commits** to the remote on GitHub.

<details><summary><b>✅ Solution</b></summary>
<br>

The solution is here exemplified with the **manta ray**. Simply replace `manta`
with your animal name.

Before starting to work, make sure we are on the correct branch:

```sh
git switch manta-dev
```

For each of the topics to populate:

* Edit the relevant topic in the `manta_ray.html` file using a text editor.
* Load/refresh the page in the browser to make sure the rendering looks good.
* Commit the changes.

```sh
# Stage the changes and create a new commit.
git add manta_ray.html
git commit -m "Manta ray: add species common and binomial name"
```

At the end of this process, we have 5 new commits on our personal branch,
that we can now push to the remote:

```sh
git push
```

</details>
<br>

### Part C - Create a pull request / merge request

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
   **Well done!** 🎉

   > **✨ Note:** it takes a few minutes before the changes are live on the
   > website.

After your Pull Request has been merged, you can update your local repository's
`main` branch with the newly added commits.

<details><summary><b>✅ Solution</b></summary>
<br>

```sh
# Update the `main` branch with the changes.
git switch main
git pull
```

</details>
<br>

### 🔮 Additional task - Repo cleanup

After your animal's branch has been merged into the `main` branch of the
project, you can now delete it from your local repo and from the remote.

1. Update your local repo with changes from the remote:

    ```sh
    git switch main
    git pull --prune
    ```

   > **✨ Note:** the **`--prune`** option in `git pull --prune`
   > removes **references to remote branches** (i.e. remote-tracking references)
   > that no longer exist on the remote. For instance, if a branch was deleted
   > on the remote (after it was merged into `main`), then you will probably
   > also want to delete your local reference to this remote branch from your
   > local copy of the repo.
   >
   > The `--prune` option can also be passed to `git fetch`.

2. Delete your branch **locally**:

    ```sh
    git branch -d <branch name>
    ```

3. Delete your branch on the **remote**:

    ```sh
    git push origin --delete <branch name>
    ```

   Alternatively, branches can also be deleted via the web interface of
   GitHub/GitLab.

<details><summary><b>✅ Solution</b></summary>
<br>

```sh
# 1. Update your local repo.
git switch main
git pull --prune

# 2. Delete your branch locally.
git branch -d manta-dev

# 3. Delete your branch on the remote.
git push origin --delete manta-dev
```

</details>
<br>
