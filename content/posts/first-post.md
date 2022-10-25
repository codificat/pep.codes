+++
title = "Renaming git branches from master to main"
author = ["Pep Turró Mauri"]
date = 2022-10-25T19:53:00+02:00
tags = ["tip", "git", "gh"]
draft = false
+++

I was looking at the [integration tests for Thoth GitHub repo](https://github.com/thoth-station/integration-tests/) and I get this warning:

> The default branch has been renamed!
>
> `master` is now named `main`
>
> If you have a local clone, you can update it by running the following commands.

These are the commands (git tips):

```sh
git branch -m master main
git fetch origin
git branch -u origin/main main
git remote set-head origin -a
```

In my case, as this is my `upstream` remote, I did:

```sh
git branch -m master main
git fetch upstream
git branch -u upstream/main main
git remote set-head upstream -a
```

This is part of Thoth Station's git branch rename effort:
[Rename the default branch in our repositories #391](https://github.com/thoth-station/core/issues/391)
