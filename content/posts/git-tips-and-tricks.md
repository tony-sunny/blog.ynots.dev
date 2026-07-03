+++
title = 'Git Tips and Tricks'
date = 2024-08-09T23:04:15+05:30
lastmod = 2024-08-09T23:04:15+05:30
draft = false
tags = ['git']
+++

## Commit fixups

If you want to make a change to a previous commit, we can do it by interactive rebase (`git rebase --interactive`).
But if we do it that way, you have to push an entirely new set of commits and if you have already submitted the changes for review, it will be hard on reviewers since they have to review the whole set again.
<!--more-->

Instead, what we can do is make changes and commit the fix separately by
```sh
git commit --fixup=<commit-id-where-the-fix-is-needed>
```

Now you can push these fixup commits separately and reviewers can just review the fix only.

And before merging, we can squash the fixup commits and keep a clean history by
```sh
git rebase --autosquash --interactive <commit_id_or_branch_name>
```

The last argument must be a commit id before all the fix up commits. If you are following feature branch based development flow, the easy thing to do is provide the target branch name where you want to merge the changes as the last argument.

Since Git 2.32, `--fixup` also accepts `amend:` and `reword:` prefixes:
```sh
git commit --fixup=amend:<commit-id> # amend the commit's message and content
git commit --fixup=reword:<commit-id> # amend only the commit's message, no content changes
```
These get squashed the same way with `git rebase --autosquash`.

## Cherry pick

Use `-x` option to append which commit was cherry-picked to commit message. \
https://git-scm.com/docs/git-cherry-pick#Documentation/git-cherry-pick.txt--x


## Stacked PRs/branches

Use `git rebase --update-refs` from the top of stack to rebase all branches. \
https://andrewlock.net/working-with-stacked-branches-in-git-is-easier-with-update-refs/

## Checking conflict markers

`git diff --check` will check if there is any conflict markers or trailing whitespaces and exit with error if present. This is useful to put as a pre commit/pre push check to prevent merging unresolved conflicts. \
Refer [documentation](https://git-scm.com/docs/git-diff#Documentation/git-diff.txt---check) for more details.
