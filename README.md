# GitHub Essentials for Engineers

Welcome! This repository is a hands-on guide to learning Git and GitHub the way engineering teams actually use it: branches, pull requests, code review, issues, and automation.

## Learning goals
By the end of this repo you should be able to:
- Clone a repo, create branches, commit changes, push to GitHub.
- Open a **Pull Request (PR)**, request review, respond to feedback, and merge safely.
- Use **Issues** for planning and bug reports (with labels and templates).
- Resolve **merge conflicts** confidently.
- Work in a team using a simple workflow.

---

# Table of Contents
1. [Setup](#setup)
2. [Git Fundamentals](#git-fundamentals)
3. [Branching & Workflow](#branching--workflow)
4. [Pull Requests & Code Review](#pull-requests--code-review)
5. [Issues & Project Planning](#issues--project-planning)
6. [Conflicts & Resolution](#conflicts--resolution)
7. [Automation - GitHub Actions](#automation--github-actions)
8. [Git Cheat Sheet](#git-cheat-sheet)
9. [FAQ / Troubleshooting](#faq--troubleshooting)

---

## Setup

### Tools you need
- Git installed (`git --version`)
- A GitHub account
- A code editor (VS Code, IntelliJ, etc.)

### First-time Git configuration
```bash
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
git config --global init.defaultBranch main
```

Following this configuration, you will be asked to authenticate with GitHub using your personal account. 

---

## Git Fundamentals

Git is a **version control system**. At a high level, it is a tool for managing a project's history, not just its files.

It tracks changes to files over time so you can:
- See what changed and why.
- Collaborate without overwriting each other.
- Undo mistakes safely.
- Understand how a project evolved.

GitHub is **not** Git — GitHub hosts Git repositories and adds collaboration features like pull requests, issues, and automation.

In practice, GitHub allows teams to:
- Work on the same codebase without overwriting each other.
- Review and discuss changes before they are merged.
- Track bugs, features, and decisions.
- Automatically test code when changes are made.

---

## Branching & Workflow

Branches allow you to work on changes **without breaking the main codebase**. Every feature, fix, or experiment should happen on its own branch.
There are many different Git branching methodologies; however, I'd recommend keeping it as simple as possible.
In this tutorial, the `main` branch represents **stable, working code**. In practice, some organisations may use a few different 'stable' branches alongside the `main` branch.

---

### A Simple Workflow

The workflow used in this repository mirrors how an example engineering team works:

1. **Create a Ticket**  
   Describe the work you want to do. GitHub offers Issues for this purpose, other examples of ticketing software include JIRA and Trello.

2. **Create a Branch from `main`**  
   All work happens on branches — never directly on `main`.

3. **Make Commits**  
   Commit small, logical changes with clear messages.

4. **Open a Pull Request (PR)**  
   Propose your changes for review.

5. **Review & Improve**  
   Respond to feedback and update your branch.

6. **Merge to `main`**  
   Only after approval and passing checks.

> **Important:** Direct commits to `main` aren't blocked by default, but it is *never* good practice to push directly to `main`. Direct commits to `main` can be blocked in Git settings.
> In most engineering teams, if it didn’t go through a Pull Request, it didn’t happen.

---

### Creating a branch

Before creating a branch, make sure `main` is up to date:

```bash
git checkout main
git pull
```

You can now create a new branch based on the current branch.
Make sure to use clear and descriptive branch naming, commonly including a ticket number.

```bash
git checkout -b feat/short-description
```

Common types
- `feat/` – New features.
- `fix/` – Bug fixes.
- `docs/` – Documentation changes.
- `task/` – Maintenance or cleanup.

---

### Working on a Branch

Standard flow, commit early and often - keep each commit focussed on an idea or deliverable:
```bash
git status                                    # Check current state
git add .                                     # Stage all changes
git add <file_name>                           # Stage specific changes
git commit -m "Add validation for input data" # Commit changes
```

---

### Pushing a Branch to GitHub

The first time you push a new branch:

```bash
git push -u origin feat/short-description
```

After that, you can use:
```bash
git push
```

---

### Keeping your Branch Updated

If your `main` branch is changing frequently, make sure to keep your local branch updated.
If your branch contains changes to files that have also changed in `main`, you will have a merge conflict. These are explained in a later section.

```bash
git checkout main                   # Switch to main
git pull                            # Get latest changes
git checkout feat/short-description # Switch to your branch
git merge origin/main               # Merge latest main into your branch
```

---

## Pull requests & Code Review

A **Pull Request (PR)** is how changes move from a branch into `main`.  
The PR will show a summary, and the changes that have been made in the branch that are being proposed for merge.
It’s also where discussion, feedback, and learning happen.

Think of a PR as:
- A proposal of changes to be made to `main`.
- A conversation about the changes and why they were made.
- A chance to get feedback and improve the code before it goes into production.

---

### When to Open a Pull Request

You can open a PR as soon as a branch has been pushed to GitHub, even as a draft if the change is not yet ready for review.
Many teams trigger automation when a PR is created, so opening a PR is often required to kick off automated test runs and quality checks.

Draft PRs are useful for early feedback and running automation without signalling that a change is ready for merge.

Once you are happy with your PR, you can mark it as `Ready for Review`, and pass the link to a team member or Slack channel for feedback.

You **do not** need the work to be perfect — PRs exist to improve it.

---

### Writing a Good Pull Request

A good PR makes life easier for reviewers. When working in a large team of engineers on a long-lived application, the quality of PRs has immense importance.
Engineers who work on your application (potentially years after your change was made) will be grateful to you for the quality of your PRs.

Include:
- **What changed** (summary)
- **Why it changed** (context or problem)
- **How it was tested**
- A link to the related issue (e.g. `Closes #12`)

From past experience, there will almost definitely be a time in your engineering life when you're looking at a code change with absolutely **NO** idea why it was made.
This is when a good quality PR is important.

#### Example PR description
```
Summary:
Updated AWS HTTP configuration to allow for better handling of sudden increases in traffic.

Why:
Due to an incident that took place on 01/01/2026, we saw a specific pattern of traffic that our application failed to handle.

Testing:
Unit tests added
Tested locally with basic cases
Application was deployed and an extended load test was carried out to ensure traffic can be sustained.
```

Smaller PRs are better:
- Easier to review.
- Easier to understand.
- Easier to fix.

**Guideline:**  
If your PR is hard to explain in a few sentences, it’s probably too big.

---

### Code Review

Code review is a vital stage in the engineering process, and is present in almost every software engineering team.
Once a change has been proposed as a PR, other engineers in the team should look through it and make comments where necessary.
Comments can be about anything from the fundamentals of the change itself, to (cue engineers arguing) code style and cleanliness.

Code review allows:
- Bugs to be caught early.
- Improves readability and design - other engineers have to read and understand your code.
- Knowledge sharing across the team - everyone will have to support your code in production.

It is **not** about:
- Personal criticism.
- Proving someone wrong.
- Rewriting everything to your taste. Everyone has an opinion on how code should be arranged but, as long as it's understandable, this should not be the focus of the review.

Having others review your code can be scary as a junior engineer, but this is where a lot of learning and development takes place. It's a great opportunity to extract knowledge and suggestions from more senior engineers.
The focus of any code review is to not only improve the quality of the proposed code, but also the quality of the engineer who produced it.

---

### How to review a Pull Request

As a reviewer:
- Read the **description first**.
- Focus on correctness, clarity and maintainability.
- Ensure the change has been covered by tests adequately.
- Ask questions if something is unclear.
- Suggest improvements and explain *why*.

**Good review comments**
- “What happens if this input is empty?”
- “Could we simplify this loop?”
- “Should this logic live in a helper function?”
- "Has this area been covered with tests?"

**Poor review comments**
- “This is bad”
- “Do it differently”
- “I don’t like this”

---

### Receiving review feedback

As the PR author:
- Feedback is about the **code**, not you
- Ask for clarification if needed
- Make changes in new commits
- Reply to comments, a back and forth over discussion points is healthy and encouraged

---

## Issues & Project Planning

**Issues** are GitHub's proposal for project tracking and management.
They act as a shared to-do list, design space, and history of decisions.

If work isn’t tracked in an issue, it’s very easy to lose context — especially in a team.
There is no requirement to use GitHub issues, alternatives include Trello, JIRA and more.
Whatever system is chosen, ensure your branches and PRs are linked to the relevant issue.

---

### What issues are used for

Issues can represent:
- Bugs
- New features
- Improvements
- Tasks or chores
- Investigations
- Questions or discussions

In real projects, issues are the starting point for almost all work.

---

## Conflicts & Resolution

Merge conflicts are a normal part of working with Git — especially when multiple people are editing the same files.  
They are not a sign that something has gone wrong.

Learning to resolve conflicts confidently is a key Git skill.

---

### What is a merge conflict?

A merge conflict happens when Git can’t automatically combine changes from two branches.

This usually occurs when:
- The same lines were edited in different branches.
- A file was changed in one branch and deleted in another.
- A branch is out of date with `main`.

Git will stop and ask **you** to decide what the correct result should be.
The longer a branch lives, the more likely conflicts become.

---

### Resolving a merge conflict (step by step)

1. **Update your local `main` branch** 
   ```bash
   git checkout main
   git pull
    ```

2. **Update your feature branch**
   ```bash
   git checkout feat/your-branch-name
   ```

3. **Merge `main` into your branch**
   ```bash
   git merge origin/main
   ```
   
4. **Open conflicting files** 

    Git will list out conflicting files, you can also see them with `git status`.
    
    Conflicts will be displayed within each file in the following format:
    ```bash
    <<<<<<< HEAD
    your changes
    =======
    changes from main
    >>>>>>> main
    ```
5. **Edit the files to resolve conflicts**

   Update each file with *exactly* how you wish the changes to be combined:
   - Keep one side
   - Combine both
   - Rewrite entirely if needed

6. **Ensure all conflict markers have been removed**

   Make sure all <<<<<<<, =======, and >>>>>>> lines are gone. These are designed to cause compile issues if left in place.

7. **Commit and push the changes**
   ```bash
   git add <file>
   git commit -m "Resolve merge conflict"
   git push
   ```
   
Once completed, your branch will be up to date with `main`, and your PR (if one exists) should no longer show conflicting files.
Make sure to check your changes to ensure your changes are as you expect them to be following the merge.

---

## Automation & GitHub Actions

GitHub Actions is a powerful automation tool built into GitHub. In brief, it allows you to define workflows of actions to take place following certain interactions with GitHub.
These interactions could be:
- A new PR is opened
- A PR is merged
- A new commit is pushed to a branch

On any of these events, we may want to run our test suite, run a linter, or deploy our application. GitHub Actions allows us to define these workflows and have them run automatically.

These checks can be added to our PR flow, and if any were to fail we could block the PR from being merged. This type of automation is vital for a large software team working at scale. Some engineers may have many changes on-going at once, and cycle between them while automated checks are run in the background.

This topic is very deep, so I won't go into detail here. More information can be found in the [GitHub Actions documentation](https://github.com/features/actions).

---

## Git Cheat Sheet

### Useful Git Commands

```bash
git clone <repo-url>                  # Clone a remote repository
git status                            # Show current state (most important command)
git log                               # Show commit history
git log --oneline                     # Compact commit history
git branch                            # List local branches
git branch -d branch-name             # Delete local branch
git add <file>                        # Stage a file
git add .                             # Stage all changes
git commit -m "message"               # Commit staged changes
git diff                              # Diff unstaged changes
git diff --staged                     # Diff staged changes
git push                              # Push commits to remote
git push -u origin branch-name        # Push new branch (first time)
git pull                              # Fetch + merge latest changes
git fetch                             # Get latest changes from remote
git merge branch-name                 # Merge branch into your branch
git restore <file>                    # Undo unstaged changes
git restore --staged <file>           # Unstage a file
git cherry-pick <commit-hash>         # Apply a commit to your branch (can be any commit in the history of any branch)
git reset --soft HEAD~1               # Undo last commit (keep changes) (useful command for when you forget something!)
git stash                             # Temporarily store changes
git stash pop                         # Restore changes from stash
git tag                               # List tags
git tag v1.0.0                        # Create a tag
git push --tags                       # Push tags to remote
```
### The .gitignore File
A `.gitignore` file tells Git which files should **never** be committed. This is useful for files that are generated locally during the development flow, and aren't required for the application to run. The `.gitignore` file is located in the root of the repository, and can contain specific file names, patterns or even directories.

**NOTE**: The `.gitignore` file is not used to ignore files that are already committed to the repository. If you want to ignore files that are already committed, you need to remove them from the repository following their addition to the `.gitignore` file.

An example file:
```bash
# Build output
dist/
build/

# Logs
*.log

# OS files
.DS_Store

# Editor files
.idea/
.vscode/
```

---

## FAQ & Troubleshooting

This section covers common questions and problems you’re likely to encounter.  
When something feels wrong, slow down — Git almost always tells you what’s happening.

### “I can’t push my changes”

Common causes:
- You haven’t committed yet
- You’re on the wrong branch
- The branch doesn’t exist on the remote yet
- Authentication failed

What to check:
```bash
git status
git branch
```

### “I committed to the wrong branch”

Don’t panic — this is fixable.

```bash
git checkout correct-branch
git cherry-pick <commit-hash> # Take your accidental commit and apply it to the correct branch
git checkout wrong-branch
git reset --hard HEAD~1 # Remove the accidental commit from the wrong branch
```