# Exam Environment Setup Notes

## General tips:
1. When generating yaml files include the question number in the filename to prevent confusion.


## .bash_profile 
```
# vi mode in terminal for hjkl movement
set -o vi

# quick kubectl aliases
alias k="kubectl"

# quick yaml generator
export dry=' --dry-run=client -o yaml'

# enables autocomplete for kubectl commands
source <(kubectl completion bash) 

# adds autocomplete to k alias
complete -o default -F __start_kubectl k

# prevents accidentally closing terminal
set -o ignoreeof 
```

## .vimrc 
```
syntax on
set ts=2 sw=2 sts=2 ai
```