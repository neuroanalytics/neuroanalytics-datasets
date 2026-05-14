# Git Introduction

Git is primarily tool for **version control**. It is a way for you
track the changes to your code in an efficient way that allows you to
more easily recover previous versions of your code without having to
save multiple variations of analyses. It is a very powerful, but also
very complicated, tool to use that will greatly improve your
reliability and confidence in working with your analysis code.

## What is will you `git` here?

This document will briefly introduce the core concepts of git and how
get started tracking your own analysis code.

### 1. Core Purpose: Version Control and Tracking Changes

Git enables the tracking of changes to files. It provides a system
that lets users **recall different versions of files**. This process
becomes more essential as collaborative projects grow.

The fundamental functions that make up the basic Git workflow include:

*   **Initialization (`init`)**: Starting a Git repository.
*   **Staging (`add`)**: Indicating that a modified file should be
	included in the next snapshot. The staging area, or Index, holds
	what will be in your next snapshot.
*   **Committing (`commit`)**: Taking a snapshot of the
	changes. Commits capture the purpose and context of changes.
*   **Inspecting**: Commands like `git status`, `git diff`, and `git
	log` are used to check the status of files, see what has changed,
	and view the history of the repository, respectively.
*   **Undoing**: Git provides ways to undo changes, such as
	unmodifying a file (`git restore <file>`) or undoing the last
	commit (`git reset HEAD~`). It also allows you to undo a commit
	from further back in history using `git revert`.

Git operates by taking a **stream of snapshots** rather than relying
on delta-based version control. It converts the contents of files and
directories into a unique, deterministic hash (e.g.,
`24b9da6552252987aa493b52f8696cd6d3b00373`), ensuring reliability.

### 2. Collaboration and Sharing Work

Git, often paired with hosting services like GitHub (which hosts Git
repositories online), is crucial for collaboration:

*   **Distributed Version Control**: Git uses a distributed model,
	meaning it does not rely solely on a central server, mitigating
	risk if the server crashes.
*   **Branching and Merging**: Git supports nonlinear development
	through **branches**, which mark a line of development (e.g., a
	new feature or a collaborator's contribution). This allows
	multiple team members to work in parallel. Git commands are used
	for starting a new branch, changing branches, and merging
	branches.
*   **Remotes**: Git is used to interact with remote repositories
	(like those on GitHub, GitLab, or Bitbucket). Commands like `git
	push` send commits to a remote repository, and `git fetch`
	retrieves commits from a remote.
*   **Handling Conflicts**: Git manages merge conflicts that arise
	when multiple people edit the same line of code, although someone
	needs to manually resolve these conflicts.
*   **Code Review and Contribution**: Git facilitates the contribution
	workflow, often involving **forks** (cloning a repository on
	GitHub) and **pull requests**. A pull request allows project
	maintainers to review code, make suggestions, and decide whether
	to merge changes into the original repository.

### 3. Project Management and Best Practices

While Git is the version control system, tools built around it (like
GitHub) use Git repositories to manage work. Best practices associated
with Git repositories help ensure effective collaboration and
streamlined workflows:

*   **Documentation**: Git repositories house documentation like the
	`README.md` file, which gives an overview of the project, setup
	instructions, and contribution guidelines.
*   **Exclusion of Files**: The `.gitignore` file, used within a Git
	repository, specifies files and directories (like build artifacts,
	sensitive information, or large media files) that should be
	excluded from version control.
*   **Maintaining History**: Git facilitates maintaining a clean
	commit history by encouraging descriptive and atomic commit
	messages.
*   **Project Organization**: GitHub allows users to classify
	repositories using **topics** (labels) to improve discoverability
	and organization based on purpose or technology stack.

Overall, Git is essential for managing code changes, enabling teams to
work collaboratively, and ensuring a recoverable history for large and
small projects.

## Creating your own repo

### Initializing a repository

Initializing a Git repository for tracking code in a project can be
accomplished through two main methods using command line Git. The
command to initialize a repository is a basic objective of the Git
workflow.

### Initialization Methods

There are two options for starting a Git repository:

1.  **Cloning an existing repository**: If the code repository already
	exists remotely (e.g., on GitHub, GitLab, or a central server),
	you can create a local copy by cloning it.
	*   Command: `git clone <repo URL>`.

2.  **Making an existing folder into a Git repository**: If you have a
	local directory containing your project files (such as those for a
	data science project) and want to start tracking changes, you can
	initialize Git in that folder.
	*   First, navigate to the desired directory: `cd <directory>`.
	*   Then, execute the initialization command: `git init`.

Once a Git repository is initialized, it includes a local repository
(the `.git/` folder) which stores metadata and snapshots.

### Essential Setup Practices for a Data Science Project

After initialization, several best practices ensure the repository is
functional and well-managed:

#### 1. Managing Files and Committing Changes

The basic workflow involves three steps after initialization:

*   **Modify:** Change a file in your working directory (the version
	of the project you are actively working on).
*   **Stage:** Indicate which modified files should be included in the
	next snapshot. You use `git add <filename>` to move changes to the
	staging area (Index). The staging area holds what will be included
	in your next snapshot.
*   **Commit:** Take the snapshot using `git commit -m “<short,
	informative commit message>”`.

When writing commit messages, they should be clear and descriptive,
explaining the purpose and context of the changes. For example, a good
commit message might be: `git commit -m "Add user authentication
mechanism to the inventory management system"`. It is bad practice to
use vague messages like `git commit -m "Fixed stuff"`.

#### 2. Utilizing `.gitignore`

It is crucial to use a **`.gitignore` file** immediately. This file
allows you to specify patterns for files and directories that should
be excluded from version control. This is particularly useful for data
science projects, as it helps avoid tracking:

*   Build artifacts (e.g., log files, temporary files).
*   Sensitive information (e.g., API keys, passwords, configuration
	files).
*   Large files (e.g., media files, binary files) that are unnecessary
	for version control. Tools like `git-lfs`, `git-annex`, or
	`datalad` can help in these scenarios.
*   Logs and caches.
*   User-specific files (e.g., editor settings).

#### 3. Documenting the Repository (README.md)

A well-documented repository is highly valuable. The **`README.md`
file** is the first item a visitor sees and should provide a quick
overview of the repository and how to get started. For projects hosted
on GitHub, the `README` supports Markdown, allowing for advanced
formatting, links, images, lists, and headers.

The `README.md` should include information such as:

*   Project description.
*   Setup and installation instructions.
*   Usage examples.
*   Contribution guidelines.
*   License information.

For a GitHub project, the `README` and project description can explain
the project's purpose and describe the project views and how to use
them, including relevant links and people to contact.

#### 4. Naming, Classification, and Documentation

If the project is hosted on GitHub, applying strong best practices
early is beneficial:

*   **Clear Repository Naming Convention:** Use a clear and
	standardized naming convention for organization and
	clarity. Examples include prefixing by project/team (e.g.,
	`teambravo_data_pipeline`), using descriptive names (e.g.,
	`machine_learning_model_trainer`), or including the technology
	stack (e.g., `image_processor_python`). This included using
	standard project structured and standards, like `cookiecutter`,
`TONIC`, `BIDS`, or `Nipoppy`.
*   **Topics:** Classify the repository using **topics** (labels) to
	help categorize and discover the project based on its purpose or
	technology stack.
*   While a README is part of the documentation, there are many tools
	to facilitate better practices that can be incorporated directly
	into your repos. Tools like `ReadTheDocs` and `MkDocs` can, with
	some additional configuration, automate the release of updated
	documentation when you commit. This advanced usage will be covered
	later in a different document.


## Working collaboratively is a challenge

Working collaboratively with Git can introduce several issues, ranging
from technical problems when integrating code to difficulty recovering
from common human errors, particularly since Git is often described as
being hard to use when things go wrong.

Key issues people might encounter when collaborating include:

### 1. Merge Conflicts

The most direct issue related to concurrent coding is the **merge
conflict**.

*   **Conflict Occurrence:** A merge conflict happens if **several
	people edit the same line of code**.
*   **Resolution Requirement:** When a merge conflict occurs (whether
	when merging branches or integrating changes between local and
	remote repositories), someone needs to **manually resolve it**.
*   **Recovering from Bad Merges:** If a merger goes wrong and breaks
	the repository, a user can try to recover using the `git reflog`
	command, which acts like a "magic time machine" to undo the damage
	and restore the repo to a previous working state.

### 2. Workflow and Communication Issues

Effective collaboration relies on standardized practices, and
deviations can lead to confusion and inefficiency. Reaching out to
maintainers and asking for clarifications on how to make contributions
is a great idea, especially for tools you heavily rely on. Every group
has different standards for how to make contributions, and until
you're experienced with the tools, you can't really know what they
are. Some things to keep in mind:

*   **Large Issues:** Breaking a large issue into smaller issues is a
	best practice, as failure to do so makes the work less manageable,
	prevents team members from working in parallel, and results in
	larger pull requests that are more difficult to review.
*   **Unclear Commit History:** Collaboration requires a clean commit
	history. This is undermined by:
	*   **Vague Commit Messages:** Using non-descriptive messages like
		`git commit -m "Fixed stuff"` is considered bad
		practice. Instead, clear and descriptive commit messages
		explaining the purpose and context of the changes are
		required.
	*   **Non-Atomic Commits:** Commits should be small, focused, and
		contain a single logical change; failing to use atomic commits
		increases the risk of conflicts and errors, and makes the
		history harder to understand.
*   **Stale Information:** To prevent information from getting out of
	sync across the project, collaborators should maintain a **single
	source of truth**. If critical data (like a target ship date) is
	tracked across multiple fields, it must be updated in all
	locations when it shifts, increasing the chance of error.

### 3. Difficulty Fixing Mistakes and Errors

Git is often perceived as difficult because "screwing up is easy, and
figuring out how to fix your mistakes is [...] impossible" if the user
does not already know the specific command needed.

*   **Amending Public Commits:** If a user realizes they need to make
	a small change right after committing, they can use `git commit
	--amend`. However, a critical issue is that one should **never
	amend commits that have been pushed up to a public/shared
	branch**. Amending public commits will lead to "a bad time".
*   **Committing to the Wrong Branch:** Accidentally committing
	changes to a shared branch (like `master`) when they should have
	been on a new branch is a common mistake. Correcting this requires
	resetting the branch history locally, which can be complex.
*   **Baffling Command Behavior:** Git commands sometimes behave
	non-intuitively for new users. For instance, if a user makes
	changes and stages them (`add`), the simple `git diff` command
	will appear empty (showing nothing changed in the working
	directory) because the changes are in the staging area. To see
	those changes, the user must remember the special flag `git diff
	--staged`.
*   **Destructive Recovery:** If a user’s branch becomes too "borked,"
	they might resort to destructive and unrecoverable actions, such
	as resetting the local state to match the remote repository
	exactly, using commands like `git reset --hard origin/master`. Be
	careful doing this, as *you risk losing your unfinished work!*

## Conclusions

Git is a complicated tool, but provides a very powerful way for you to
more easily adhere to best research practices when working with your
code. It is very much worth the upfront investment to learn this
ecosystem. Fortunately, because it is so powerful, there are many
great resources for getting started with it. Please feel free to find
and contribute or recommend additional information to improve this
document!

## References

- https://docs.github.com/en/get-started/start-your-journey/about-github-and-git
- https://ohshitgit.com/
- https://github.com/neurodatascience/course-materials-2024/blob/ac3f7220a320fb234ced226ddf1c1acce25c2ee0/2024/03_git_github/lecture/git_github__lecture_slides.pdf
