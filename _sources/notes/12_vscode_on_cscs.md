# Using vscode on CSCS for MCH

## General

1. Make sure you are connecting to only one of tsa-ln002 or tsa-ln003 servers. Therefore, ssh xyz@tsa-ln002.cscs.ch
2. Within VScode, disable telemetry
3. in Remote.SSH settings, enable Remote.SSH: Remote Server Listen on Socket in VS Code

## For fortran

1. note that your local extensions may not be active on remote host. They may have to re-installed
2. install modern fortran and fortran intellisense. For fortran intellisense, you need the fortran language server. For this, use

```
pip3 install fortran-language-server --user
```
