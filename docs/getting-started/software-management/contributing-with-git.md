# Contributing with Git

## Prerequisites

### Terms

`git`

:   A version control system for managing an application's codebase, the code's history, and various versions and contributions to the code.

`repository (repo)`

:   TODO: define repo

`Git Host`

:   TODO: define Git Host.  The USGS typically uses GitHub (or in some cases GitLab).

`repo's page`

:   The webpage for a repository on a Git Host.

### Programming Language

Before contributing, you should have a basic understanding of the language a program is written in.  For example, ISIS is written in c++, and ALE is written in Python.

### Installing Git

If you don't have git on your computer, you will need to install it first.  You can run `git version` in your terminal to check if git is installed.  If it is not, install as appropriate for your system.

| System  | Installation                                                                                                                           |
|---------|----------------------------------------------------------------------------------------------------------------------------------------|
| Windows | Download and install git for Windows from the [git-scm website (external)](https://git-scm.com/download/win).                          |
| Mac     | Mac installs git automatically when you run it from the terminal.  If you have Homebrew, you may install git via `brew install git`.   |
| Linux   | Install git using your package manager. For example, `sudo apt install git` on Ubuntu.                                                 |

Once git is installed, set your username and email:

    git config --global user.name "Your Name"
    git config --global user.email "emailaddress@example.com"

## Issues

![Issue board on GitHub](../../assets/software-management/git-issue-board.png){ align=right width=450 }

Your contribution should be related to an issue.  You can browse a list of issues on the repository page for each project.  If you are working on an issue you have found yourself, and the issue is not listed yet, please [report the issue](../../how-to-guides/software-management/guidelines-for-reporting-issues.md).

## Fork

A fork is a copy of a repository at a particular time.  The repo that has been forked from is usually called "upstream", and your fork/working copy is called "origin" by default.

Your fork is your own working copy, where you can test your code or add new features.  While you are working on your contribution, you don't have to worry about unfinished code affecting the upstream repo; you are on your own fork.  Later, when your code is done, there is a review process before your code can be merged into the upstream repo.

TODO: Add image
![Forking a repository on GitHub](){ align=right width=450 }

To make a fork, click the fork button on the repository page, and then enter a name for your fork.  If you want to contribute back to the original repo, you can just leave the name the same (your fork is differentiated by your username).

## Clone

TODO: Add image
![Cloning a repository on GitHub](){ alight=right width=450 }

To download the repo to your computer:
1. Click the clone button on the repo's page.
1. Copy the address of the repo.
   - If you use the SSH address, you will need to [set up SSH keys on your computer and in your git account (external link)](https://docs.github.com/en/authentication/connecting-to-github-with-ssh).
1. In your terminal, navigate to a parent directory of choice for your various repos (commonly name "source", "repos", or "code")
1. Run the `git clone <your_repo_address>` command.  When it is done, you will have a subdirectory with your repo in it.

## Branch

## Make Changes

## Test

## Commit

## (Sync and) Push

## Create Pull Request