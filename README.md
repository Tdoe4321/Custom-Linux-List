# Custom-Linux-List
A list of personal customization items that I like for Linux

### Useful Programs
**Changing rm to move things to a trashcan**
```bash
sudo apt-get install trash-cli
sudo vim /usr/local/bin/trash-rm
```

paste the following in that new file:

```bash
#!/bin/bash
# command name: trash-rm
shopt -s extglob
recursive=1
declare -a cmd
((i = 0))
for f in "$@"
do
case "$f" in
(-*([fiIv])r*([fiIv])|-*([fiIv])R*([fiIv]))
tmp="${f//[rR]/}"
if [ -n "$tmp" ]
then
#echo "\$tmp == $tmp"
cmd[$i]="$tmp"
((i++))
fi
recursive=0 ;;
(--recursive) recursive=0 ;;
(*)
if [ $recursive != 0   -a  -d "$f" ]
then
echo "skipping directory: $f"
continue
else
cmd[$i]="$f"
((i++))
fi ;;
esac
done
trash-put "${cmd[@]}"
```

```bash
sudo chmod +x /usr/local/bin/trash-rm && \
echo ' ' >> ~/.bashrc && \
echo '# Changes rm to go to trashcan, not delete forever' >> ~/.bashrc && \
echo 'alias rm="trash-rm"' >> ~/.bashrc
```

### .bashrc additions
merges all commands (from all terminals) immediatly into ~/.bash_history but keeps individual 'up' history individual
```bash
# Merges all terminal history into ~/.bash_history
shopt -s histappend
PROMPT_COMMAND="history -a;$PROMPT_COMMAND"
```

Shows file size for all files and folders in current directory
```bash
# Shows file size
alias df="du -sh *"
```
Colors man pages nicely:
```bash
# Pretty man colors
export LESS_TERMCAP_mb=$'\E[01;31m'
export LESS_TERMCAP_md=$'\E[01;31m'
export LESS_TERMCAP_me=$'\E[0m'
export LESS_TERMCAP_se=$'\E[0m'
export LESS_TERMCAP_so=$'\E[01;44;33m'
export LESS_TERMCAP_ue=$'\E[0m'
export LESS_TERMCAP_us=$'\E[01;32m'
```

### .vimrc additions
Much like how we have a .bashrc that is executed when a new bash shell is instanciated, we have a .vimrc that is executed when we open a file in vim. However, this file may not exist when you install vim. If it doesn't, you will need to add it to your home directory:
```bash
touch ~/.vimrc
```

Show line numbers:
```bash
set nu
```


### Git stuff
The aliases below are useful for Git commands.  Note that these must be implemented in your .gitconfig file (likely located in your home directory).

```bash
[alias]
    hist = log --pretty=format:\"%h %ad | %s%d [%an]\" --graph --date=iso
    histf = log --pretty=format:\"%h %ad | %s%d [%an]\" --graph --date=iso --name-status
    histc = log --graph --pretty=oneline --abbrev-commit
```
