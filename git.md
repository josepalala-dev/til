# Git Worktrees: A 5-Minute Guide

## 1. What is a worktree?

A Git worktree lets you work on multiple branches at the same time, in separate folders, using the same repository.

Normally, switching branches changes the files in your current folder. With worktrees, each branch gets its own working directory.

- Without worktrees: One folder, switch branches to change tasks.
- With worktrees: Multiple folders, each checked out to a different branch.

## 2. Create your first worktree

Suppose you already have a project on the `develop` branch.

```
cd my-project
```

Create a new folder for a feature branch:

```
git worktree add ../my-project-login -b feature/login
```

Git creates the `feature/login` branch and checks it out in the new folder.

Your directories now look like this:

```
projects/
├── my-project/          # develop
└── my-project-login/    # feature/login
```

Both folders belong to the same Git repository. You don't need to clone the repository again.

## 3. Work on multiple tasks

Imagine you're developing a Node.js application.

- `my-project/`: Continue your current work on `develop`.
- `my-project-login/`: Implement and test the login feature.
- `my-project-hotfix/`: Fix an urgent production bug.

Create a hotfix worktree from `main`:

```
git worktree add ../my-project-hotfix -b hotfix/payment main
```

You can open each folder in a separate VS Code window:

```
code .
code ../my-project-login
code ../my-project-hotfix
```

Each window has its own files and working directory. You can edit and test different tasks without switching branches in the original folder.

## 4. How do commits work?

Worktrees share the same Git repository and commit history, but each has its own working files and index.

For example, from your feature worktree:

```
cd ../my-project-login

git add .
git commit -m "feat: add login"
```

The commit belongs to the shared repository and the `feature/login` branch. Your original `develop` worktree remains checked out on `develop`.

When the feature is ready, merge it into `develop` using your team's normal workflow.

## 5. Five commands to remember

| Command                                      | Purpose                                      |
| -------------------------------------------- | -------------------------------------------- |
| `git worktree list`                          | List all worktrees                           |
| `git worktree add ../folder -b feature/name` | Create a folder and new branch               |
| `git worktree add ../folder existing-branch` | Check out an existing branch in a new folder |
| `git worktree remove ../folder`              | Remove a worktree                            |
| `git worktree prune`                         | Clean up stale worktree metadata             |

To remove a worktree:

```
git worktree remove ../my-project-login
```

Commit or preserve any work you need first. Removing a worktree deletes its working directory, but does not automatically delete its branch.

## 6. Important rules

1. A branch can normally be checked out in only one worktree at a time. Create another branch if you need a separate checkout.
2. Uncommitted changes stay in their own worktree. They do not automatically appear in other worktrees.
3. Commits are shared. All worktrees use the same repository history.
4. Dependencies may need separate installation. For example, each Node.js worktree may need its own `node_modules`.
5. Worktrees are not independent clones or backups. They share Git metadata, so deleting or damaging the main repository can affect all worktrees.

## The key idea

A worktree is a separate working folder for a branch, backed by the same Git repository.

Use it when you want to work on a feature, investigate a bug, or prepare a hotfix without interrupting your current task.
