# Custom-Linux-List
A list of personal customization items that I like for Linux

### Useful Programs
**Changing rm to move things to a trashcan**
```
sudo apt-get install trash-cli
sudo vim /usr/local/bin/trash-rm
```

paste the following in that new file:

```
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

```
sudo chmod +x /usr/local/bin/trash-rm
echo 'alias rm="trash-rm"' >> ~/.bashrc
bash
```

### .bashrc additions
```
alias df="du -sh *" | #Shows file size for all files and folders in current directory
```

### Git stuff
The aliases below are useful for Git commands.  Note that these must be implemented in your .gitconfig file (likely located in your home directory).

```bash
[alias]
    hist = log --pretty=format:\"%h %ad | %s%d [%an]\" --graph --date=iso
    histf = log --pretty=format:\"%h %ad | %s%d [%an]\" --graph --date=iso --name-status
    histc = log --graph --pretty=oneline --abbrev-commit
```
