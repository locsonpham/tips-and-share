# Git Tips

## Display branch without paging

### Git branch pager
**Problem**
git branch show output in new screen, requires 'q' to close and you forgot your branch outputs.

**Solved**
`
git config --global pager.branch false
`

## Fetch remote branches and remove deleted remote branches
`
git fetch -p
git fetch --prune

## Submodule
Remove sub module
`
$ git submodule deinit the_submodule
$ git rm the_submodule 
`

To remove a submodule you need to:

Delete the relevant section from the .gitmodules file.
Stage the .gitmodules changes git add .gitmodules
Delete the relevant section from .git/config.
Run git rm --cached path_to_submodule (no trailing slash).
Run rm -rf .git/modules/path_to_submodule (no trailing slash).
Commit git commit -m "Removed submodule "
Delete the now untracked submodule files rm -rf path_to_submodule