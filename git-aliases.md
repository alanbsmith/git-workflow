# git aliases

```
alias amend="git commit --amend" # amend commit

alias gcm="git commit -m" # commit with message

alias ga="git add -A" # add everything

alias gs="git status" # git status

alias gas="git add -A; git status" # git add everything 
and show status

alias gcb="git checkout -b" # checkout a new branch

alias gb="git branch" # view local branches

alias gba="git branch -a" # view all branches

alias last="git log -1 --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit" # fancy commit log

alias gpom="git pull origin master" # pull down master

alias master="git checkout master" # switch to master branch

alias staging="git checkout staging" # switch to staging 
branch

alias gb-="git checkout -" # switch to previous branch

alias grm="git rebase master" # rebase you branch with master

alias rebase="git rebase -i" # interactive rebase

alias grh='git reset --hard' # reset all non-committed changes

alias push='git push origin HEAD' # push current branch

alias pull='git pull' # pull current branch"
```