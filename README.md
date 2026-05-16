# opencode-install

This repository contains a customized copy of the `install` script from the upstream [opencode](https://github.com/anomalyco/opencode) project.

The repository was created using `git filter-repo --path install`, so it contains:

* Only the `install` file.
* The full history of that file.
* My own custom commits.

## Usage

```txt
OpenCode Installer

Usage: install.sh [options]

Options:
    -h, --help              Display this help message
    -v, --version <version> Install a specific version (e.g., 1.0.180)
    -b, --binary <path>     Install from a local binary instead of downloading
        --no-modify-path    Don't modify shell config files (.zshrc, .bashrc, etc.)

Examples:
    ./install
    ./install --version 1.0.180
    ./install --no-modify-path
    ./install --binary /path/to/opencode
```

## Summary

This repository tracks only the upstream `install` file while preserving your custom changes.

To update:

1. Fetch the upstream `dev` branch.
2. Extract only the `install` file.
3. Merge the extracted history into your branch.
4. Resolve conflicts if necessary.
5. Push the updated result.

This workflow gives you a minimal repository with the full history of the file and a straightforward process for incorporating upstream changes.

## Configure the Upstream Remote

Run this once after cloning the repository.

```bash
git remote add upstream git@github.com:anomalyco/opencode.git
```

Verify:

```bash
git remote -v
```

Expected output:

```text
origin      git@github.com:Farid-NL/opencode-install.git (fetch)
origin      git@github.com:Farid-NL/opencode-install.git (push)
upstream    git@github.com:anomalyco/opencode.git (fetch)
upstream    git@github.com:anomalyco/opencode.git (push)
```

## Update from Upstream

The following steps fetch the latest changes from the upstream `dev` branch,
re-extract only the `install` file, and merge those changes into your repository
while preserving your custom modifications.

### 1. Fetch the Latest Upstream Changes

```bash
git fetch upstream dev
```

### 2. Create a Temporary Worktree

```bash
git worktree add ../upstream-install upstream/dev
```

### 3. Filter the Worktree to Keep Only `install`

```bash
cd ../upstream-install
git filter-repo --path install --force
git checkout -b upstream-install
```

### 4. Return to Your Repository

```bash
cd -
```

### 5. Add the Temporary Repository as a Remote

```bash
git remote add filtered ../upstream-install
git fetch filtered upstream-install
```

### 6. Merge Upstream Changes

```bash
git merge filtered/upstream-install
```

At this point, one of three things will happen:

* **Already up to date** → No upstream changes.
* **Merge commit created** → Changes were merged successfully.
* **Merge conflicts** → Manual resolution is required.

### 7. Clean Up

```bash
git remote remove filtered
git worktree remove ../upstream-install
```

### 8. Push to GitHub

```bash
git push origin dev
```

## Check What Changed Upstream

Before merging, you can inspect the differences:

```bash
git diff HEAD filtered/upstream-install -- install
```

Show commits that changed the file:

```bash
git log --oneline --left-right HEAD...filtered/upstream-install -- install
```

## Monthly Update Workflow

Run these commands periodically:

```bash
git fetch upstream dev
git worktree add ../upstream-install upstream/dev
cd ../upstream-install
git filter-repo --path install --force
git checkout -b upstream-install
cd -
git remote add filtered ../upstream-install
git fetch filtered upstream-install
git merge filtered/upstream-install
git remote remove filtered
git worktree remove ../upstream-install
git push origin dev
```

## Install `git-filter-repo`

### Fedora

```bash
sudo dnf install git-filter-repo
```

### Verify

```bash
git filter-repo --help
```
