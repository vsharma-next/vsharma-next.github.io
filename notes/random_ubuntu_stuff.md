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
