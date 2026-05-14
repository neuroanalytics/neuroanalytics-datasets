# Git and Github Example Usage

A simple workflow using Git to track changes, make commits to a file,
and push those changes to GitHub involves initializing a repository,
utilizing the three-step basic workflow (modify, stage, commit), and
interacting with the remote repository.

This tutorial focuses on working with Git through the command line,
which helps in understanding how Git operates.

## An Example Commit

### I. Setup and Initialization

First, you need a Git repository. You have two primary options for
starting a repository:

1.  **Clone an existing repository (e.g., from GitHub):** `git clone
	<repo URL>`

2.  **Make an existing local folder into a Git repository:** Start by
	navigating to the directory (`cd <directory>`) and then
	initializing Git: `git init`

### II. Tracking and Committing Changes Locally

The basic Git workflow involves three steps: Modify, Stage, and
Commit. Changes occur in the **working directory**, are prepared in
the **staging area**, and are stored as a **snapshot** in the local
repository.

| Step | Action | Description | Command | Source |
| :--- | :--- | :--- | :--- | :--- |
| **1. Modify** | Change a file | You make a change to a file in your working tree. | *Edit your code file.* | |
| **Inspect** | Check status/diff | Check the status of the files to see what is modified, and see what changed since the last commit. | `git status` <br> `git diff` | |
| **2. Stage** | Prepare the file | You indicate that you want the modified file included in your next snapshot (moving it to the staging area/index). | `git add <filename>` | |
| **3. Commit** | Take the snapshot | You take the snapshot, storing the changes in the local repository. This requires a short, informative commit message. | `git commit -m “<short, informative commit message>”` | |

A good practice is to write **descriptive commit messages** that
explain the purpose and context of the changes. For instance, avoiding
vague messages like `"Fixed stuff"` is recommended.

### III. Pushing to GitHub (Remote)

Once you have committed your changes locally, you need to share your
work by pushing it to the remote repository (GitHub).

1.  **Check/Add Remote Repository:** If you didn't clone the
	repository (and therefore didn't automatically configure the
	remote), you may need to add it. You can view existing remotes
	using: `git remote -v` To add a new remote repository: `git remote
	add <remote name> <remote address>`

2.  **Push Commits:** To upload your commits to the remote repository,
	use the `push` command: `git push <remote name> <branch>`

This action sends your local snapshots to the online host (GitHub).

## An example merge conflict

A merge conflict arises in Git when **several people edit the same
line of code**.

If a merge conflict occurs, the sources indicate that the basic
process necessitates **manual resolution** by a user.

Although the objective of learning Git often includes understanding
"how to solve a merge conflict" and managing a "merge conflict between
your local and remote repository", the sources emphasize that
resolving the clash requires a human intervention.

For example, if two branches attempt to merge changes into a file like
`colors.txt`, where one branch has defined a line as `red blue` and
the other has defined it as `red green`, a conflict marker would
appear, requiring manual editing to decide the final, correct content
(e.g., just `red`).

The basic process of resolving a merge conflict requires **manual
intervention** to decide which lines of code should be kept. Once the
conflict is manually resolved within the file, the user employs
standard Git workflow commands to finalize and record the resolution.

While the sources emphasize that the resolution is a manual effort by
a person, the commands surrounding the fix include:

### 1. Inspection and Initial Status

Before starting the fix, the user can check which files are in a
conflicted state:

*   **`git status`**: Used to check the status of files in the
	repository. This command will indicate that a merge conflict has
	occurred and show the files that need resolution.

### 2. Manual Resolution

The conflict itself is resolved by opening the conflicted file and
removing the conflict markers (which typically look like `<<<<<<<`,
`=======`, and `>>>>>>>`), choosing the correct content, and deleting
the incorrect content.

### 3. Staging and Committing the Resolution

After the file is manually edited, the modified file must be staged
and committed:

*   **`git add <filename>`**: This command is used to stage the
	modified file, indicating that the user wants the resolved version
	of the file included in the next snapshot.
*   **`git commit -m “<short, informative commit message>”`**: Once
	the resolution is staged, this command is used to take the final
	snapshot, storing the successful merge resolution in the local
	repository.

### 4. Recovery from a Bad Merge

If the user attempts a merge, resolves the conflict poorly, or breaks
the repository, they can use specific commands to step back in time:

*   **`git reflog`**: This command is useful because it shows a list
	of nearly every action the user has done in Git across all
	branches.
*   **`git reset HEAD@{index}`**: After using `git reflog` to identify
	the state of the repository *before* the merge went wrong, this
	command acts as a "magic time machine" to undo the last committed
	actions, allowing the user to recover after a bad merge.

## Branches

Branches, which mark a line of development, contribute to making work
manageable and help to reduce the risks associated with merge
conflicts primarily by promoting structured, isolated, and smaller
sets of changes.

Here is how branching strategies assist in dealing with conflicts:

1.  **Reducing the Risk of Conflicts:** Adopting a **consistent
	branching strategy** is critical for effective code management and
	collaboration. Following such a strategy helps to maintain a clean
	and stable codebase and, most importantly, **reduces the risk of
	conflicts and errors**.

2.  **Enabling Parallel and Isolated Work:** Branches facilitate
	working on changes nonlinearly. Best practices encourage
	**breaking down large issues into smaller issues**. This breakdown
	makes the overall work more manageable and **enables team members
	to work in parallel**. When parallel work is done on separate
	branches (such as using Feature Branching, where each task gets a
	dedicated branch), developers are less likely to constantly modify
	the exact same lines of code simultaneously, which is the cause of
	a merge conflict.

3.  **Creating Smaller Changes:** Breaking down large issues results
	in **smaller pull requests** (PRs), which are easier to
	review. These smaller, more focused changes minimize the scope of
	the potential differences when integrating the work back into the
	main codebase, helping to manage the complexity of manual conflict
	resolution.

## Collaboratively Working on a Repository: Pull Requests

The basic process for a user to make a Pull Request (PR) from their
branch to another repository, particularly in a collaborative setting,
revolves around using a **fork**.

This process focuses on the **Remotes - collaborating/contributing**
workflow:

### 1. Working in a Fork
In this collaborative model, most collaborators will utilize their
**own fork** to develop their contributions. A fork is essentially
"like a clone, but on GitHub".

### 2. Pushing Commits
Once the user has committed their changes locally on their branch,
they must **push their commits to their own fork**. This makes the
changes available on the remote repository (their fork on GitHub, or
similar host).

### 3. Opening the Pull Request
After the commits are pushed to the user's fork, the user then
**open[s] a pull request**.

### 4. Review and Decision
The Pull Request acts as a formal proposal to incorporate the changes
into the original repository.

Upon opening the PR:
*   Project maintainers can then **review the code**.
*   Maintainers can **make suggestions or edits**.
*   They ultimately **decide whether to merge it into the original
	repository**.

Best practices suggest that breaking down large issues into smaller
issues results in **smaller pull requests**, which are generally
**easier to review**. Additionally, Pull Requests include built-in
features for communication, such as using **$@$mentions** to alert
collaborators.

GitHub Projects automatically stay up to date with GitHub data,
including information from pull requests. Automated workflows, such as
those using GitHub Actions, can also be configured to automatically
add a pull request to a project and set its status (e.g., to "needs
review") when it is marked as "ready for review".
