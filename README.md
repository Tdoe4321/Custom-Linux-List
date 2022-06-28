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

Git status alias
```bash
# Git status alias
alias gits="git status"
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
# Shows current branch
```bash
parse_git_branch() {
 git branch 2> /dev/null | sed -e '/^[^*]/d' -e 's/* \(.*\)/(\1)/'
}
PS1='${debian_chroot:+($debian_chroot)}\[\033[01;32m\]\u@\h\[\033[00m\]:\[\033[01;34m\]\w\[\033[01;31m\]$(parse_git_branch)\[\033[00m\]\$ '
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

### VS Code Keybindings
Add to keybindings.json & install required extensions
```
// Place your key bindings in this file to override the defaults
[
    // Switch between terminal tabs
    { "key": "alt+[ArrowDown]", "command": "workbench.action.terminal.focusNext", "when": "terminalFocus" },
    { "key": "alt+[ArrowUp]", "command": "workbench.action.terminal.focusPrevious", "when": "terminalFocus" },
    
    // Close terminal tab
    { "key": "ctrl+w", "command": "workbench.action.terminal.kill", "when": "terminalIsOpen && terminalFocus || terminalProcessSupported && terminalFocus" },
    
    // Fullscreen current code tab + remove terminal
    // REQUIRES multicommand and clever extensions
    // ALSO: remove default ctrl+shift+x: File->Preferences->Keyboard Shortcuts
    {
        "key": "ctrl+shift+x",
        "command": "extension.multiCommand.execute",
        "args": {
            "sequence": [
                "clever.maximize.toggleWithoutSidebar"
            ]
        },
    },
    {
        "key": "ctrl+shift+x",
        "command": "-workbench.view.extensions",
        "when": "viewContainer.workbench.view.extensions.enabled"
    },
    { "key": "ctrl+shift+z", "command": "workbench.action.togglePanel"},
    
    // Send Current tab to second view
    { "key": "ctrl+shift+e", "command": "workbench.action.moveEditorToNextGroup", "when": "editorFocus" },
    
    // Send Current tab to first view
    { "key": "ctrl+shift+r", "command": "workbench.action.moveEditorToPreviousGroup", "when": "editorFocus" },

    // Switch between open tabs in current group
    {
        "key": "alt+[ArrowRight]",
        "command": "workbench.action.nextEditor", "when": "editorTextFocus"
    },
    {
        "key": "alt+[ArrowLeft]",
        "command": "workbench.action.previousEditor", "when": "editorTextFocus"
    },

    // Move tabs in order
    {
        "key": "alt+shift+[ArrowRight]",
        "command": "workbench.action.moveEditorRightInGroup", "when": "editorTextFocus"
    },
    {
        "key": "alt+shift+[ArrowLeft]",
        "command": "workbench.action.moveEditorLeftInGroup", "when": "editorTextFocus"
    },


    // Page down/up on ctrl+shift+down/up
    {
        "key": "ctrl+shift+up",
        "command": "cursorMove",
        "args": {
            "to": "up",
            "by": "line",
            "value": 10
        },
        "when": "editorTextFocus"
    },
    {
        "key": "ctrl+shift+down",
        "command": "cursorMove",
        "args": {
            "to": "down",
            "by": "line",
            "value": 10
        },
        "when": "editorTextFocus"
    },
    
    // Jump between terminal and text editor
    { "key": "alt+z", "command": "workbench.action.terminal.focus", "when": "editorTextFocus"},
    { "key": "alt+z", "command": "workbench.action.focusActiveEditorGroup", "when": "terminalFocus"},
    
    // Keyboard navigate backwards and forwards
    {
        "key": "ctrl+shift+a",
        "command": "workbench.action.navigateBack",
        "when": "editorTextFocus"
    },
    {
        "key": "ctrl+shift+s",
        "command": "workbench.action.navigateForward",
        "when": "editorTextFocus"
    }
    
    // Terminal commandd
    {   // Split terminal view
        "key": "ctrl+shift+e",
        "command": "workbench.action.terminal.split",
        "when": "terminalFocus && terminalProcessSupported || terminalFocus && terminalWebExtensionContributedProfile"
    },
    {   // Make new terminal tab
        "key": "ctrl+shift+n",
        "command": "workbench.action.terminal.new",
        "when": "terminalFocus && terminalProcessSupported || terminalFocus && terminalWebExtensionContributedProfile"
    },

    // Resizing terminal
    {
        "key": "ctrl+shift+left",
        "command": "workbench.action.terminal.resizePaneLeft",
        "when": "terminalFocus && terminalProcessSupported || terminalFocus && terminalWebExtensionContributedProfile"
    },
    {
        "key": "ctrl+shift+right",
        "command": "workbench.action.terminal.resizePaneRight",
        "when": "terminalFocus && terminalProcessSupported || terminalFocus && terminalWebExtensionContributedProfile"
    },
    {
        "key": "ctrl+shift+up",
        "command": "workbench.action.terminal.resizePaneUp",
        "when": "terminalFocus && terminalProcessSupported || terminalFocus && terminalWebExtensionContributedProfile"
    },
    {
        "key": "ctrl+shift+down",
        "command": "workbench.action.terminal.resizePaneDown",
        "when": "terminalFocus && terminalProcessSupported || terminalFocus && terminalWebExtensionContributedProfile"
    }

]
```

To allow the ctrl+b sidebar command to work while the focus is in the terminal, use the following:
Search for terminal profile in settings -> "edit in settings.json" for windows. Add the following:
```
"terminal.integrated.commandsToSkipShell": [
        "workbench.action.toggleSidebarVisibility"
]
```

To allow alt not to be used for top bar navigation, search for 'settings' and 'Open Default Preferences' should come up. Add the following:
```
"window.titleBarStyle": "custom",
"window.customMenuBarAltFocus": false
```
