# Git Tips

## Display branch without paging

### Git branch pager
**Problem**
git branch show output in new screen, requires 'q' to close and you forgot your branch outputs.

**Solved**
`
git config --global pager.branch false
`