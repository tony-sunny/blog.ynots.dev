+++
title = 'Git Tips and Tricks'
date = 2024-08-09T23:04:15+05:30
lastmod = 2024-08-09T23:04:15+05:30
draft = false
+++

## Commit fixups

If you want to make change to previous commit, we can do it by interactive rebase(`git rebase --interactive`).
But if we do it that way, you have to push an entirely new set of commits and if you have already submitted the changes for review, it will be hard on reviewers since they have to review the whole set again.
<!--more-->

Instead, what we can do is make changes and and commit the fix separately by
```sh
git commit --fixup=<commit-id-where-the-fix-is-needed>
```

Now you can push these fixup commits separately and reviewers can just review the fix only.

And before merging, we can squash the fixup commits and keep a clean history by
```sh
git rebase --autosquash --interactive <commit_id_or_branch_name>
```

The last argument must be a commit id before all the fix up commits. If you are following feature branch based development flow, the easy thing to do is provide the target branch name where you want to merge the changes to as athe last argument.
