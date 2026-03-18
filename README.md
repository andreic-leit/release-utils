# release-utils
## Description

dataops release utils.

## git
git related commands 
### Release commands

- lists only the filenames changed in the commit with `git diff-tree`

```shell

git diff-tree --no-commit-id --name-only -r <commit_id>

```

### Merge commands

```shell

git merge --no-commit --no-ff <branch>

```