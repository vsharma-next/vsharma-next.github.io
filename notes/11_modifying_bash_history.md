# Enhanching bash history setup

Source: https://www.thomaslaurenson.com/blog/2018-07-02/better-bash-history/

## Adding further notes

1. I added this to my /etc/bash.bashrc
2. extended /etc/bash.bashrc is

```
# Print the timestamp of each command
HISTTIMEFORMAT='%F %T '

# Save 100000 lines of history in memory
HISTSIZE=100000
# Save 2,000,000 lines of history to disk (will have to grep ~/.bash_history for full listing)
HISTFILESIZE=2000000
# Do not store a duplicate of the last entered command
HISTCONTROL=ignoredups
# Ignore more
HISTIGNORE='ls:ll:ll -htr:clear:history'

# Configure BASH to append (rather than overwrite the history):
shopt -s histappend

# Attempt to save all lines of a multiple-line command in the same entry
shopt -s cmdhist

# save multi-line commands to the history with embedded newlines
shopt -s lithist

# After each command, append to the history file and reread it
export PROMPT_COMMAND="${PROMPT_COMMAND:+$PROMPT_COMMAND$"\n"}history -a; history -c; history -r"

```

3. make sure you are using sudo vim ...

4. Added the same edits to .bashrc on mch/daint
