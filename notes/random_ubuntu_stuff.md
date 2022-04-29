# random snippets for ubuntu

## cleaning up apt (CMDLINE)

to remove a repo from apt sources,

1. remove entry from `/etc/apt/sources.list`. The entry should look like `deb http://ch.archive.ubuntu.com/ubuntu/ focal multiverse`
2. delete repo keys

- `sudo apt-key list`
- get the 40 character long HEX value from APT
- use sudo apt-key del "PUT HEX VALUE HERE" - for example `sudo apt-key del "3820 03C2 C8B7 B4AB 813E 915B 14E4 9429 73C6 2A1B" `

## adding stuff to startup (GUI)

- open startup application
- add just the command you want executed at startup ( the same command that you would type if you were running the app from thhe terminal )

## grep

- to return the word containing a search string (and not the entire line)

```
grep -i -r -o '\w*itype\w*' ./sfc_terra.f90
```

returns all words with itype in it.

- return only unique values

```
grep -i -r -o '\w*itype\w*' ./sfc_terra.f90 | sort | uniq
```

returns only unique values from the match

## sudo apt

- to suppress output and assume yes to all imports

```
sudo apt-get -yqq instal xxxyyyzzz
```

## Enhanching bash history setup

Source: https://www.thomaslaurenson.com/blog/2018-07-02/better-bash-history/

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
