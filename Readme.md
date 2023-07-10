* Microsoft Windows 11 Pro 22H2 (22621.1848)
* ssh-keygen -t ed25519 -C 'mz@pug'
  * PS C:\Users\mz0> type C:\Users\mz0/.ssh/id_ed25519.pub
    ```
    ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIA3MKStmrL917EbmLfM52LnmRiAmF5uhlYh7NAXvjRId mz@pug
    ```
  * [Exactpro Gitlab](https://gitlab.exactpro.com/-/profile/keys) / [Github](https://github.com/settings/keys)
    (Gitlab - remove "Expiration date")

* enable symlinks
  * "Computer Management" - "Local Users and Groups" - "Groups" - New Group
    * Group name: developer (Description: can make symlinks)
    * add your account to this group (e.g. for Microsoft account mz0@outlook.com type mz0 and "Check Names")
  * "Local Security Policy" - "Run as Administrator"
    * Local Policies-> User Rights Assignment -> Create symbolic links (initially has only "Administrators" built-in group)
    * Add User or Group (Object Types - check 'Groups') - "developer" - "Check Names"

* `winget install --interactive Git.Git`
  * UAC confirmation
  * accept GPL
  * Select Destination Location: C:\t\Git
  * (screenshot)
  * Select Start Menu Folder - Next
  * Choosing the default editor used by Git (`core.editor`)
  * initial branch name (main/master)
  * PATH - Use Git and optional Unix tools (IMPORTANT - we need `diff`)
  * use external OpenSSH
  * HTTPS transport - Use native Windows Secure Channel library
  * line ending conversion Checkout & commit as-is (`core.autocrlf false`)
  * Use Windows' default console window
  * 'git pull' behavior - Default (fast-forward)
  * credentials helper - None
  * extra options - Enable symbolic links [see note below]
  * experimental options - up to you - Install -- Finish
  * open "Bash prompt"
    * `git config --global core.symlinks true`
    * Are you sure you want to continue connecting (yes/no/[fingerprint])? - type yes (no echo)
```
git clone git@gitlab.exactpro.com:vivarium/shsha/shsha.git
Cloning into 'shsha'...
remote: Enumerating objects: 87473, done.
remote: Counting objects: 100% (87473/87473), done.
remote: Compressing objects: 100% (21873/21873), done.
fetch-pack: unexpected disconnect while reading sideband packet
fatal: early EOF
fatal: fetch-pack: invalid index-pack output

git config --global core.compression 0
```
https://stackoverflow.com/questions/21277806/fatal-early-eof-fatal-index-pack-failed

This error may occur for memory needs of git.

On symlink "support"

* `git config --global core.symlinks true` (or better yet at repository level) is unavoidable at least in the original installer's
  session (no idea if it has any effect on the 'Default User profile')
  `git clone -c core.symlinks=true ...` is also an option for initial repo cloning
* Git Bash ``ln [-s]` doesn't make a [sym]link, it just makes a copy of file
* Windows way of making symlink is (in CMD.EXE only) `mklink Dst Src` (see [mklink](https://ss64.com/nt/mklink.html))
  Note: `MKLINK` is a CMD.EXE built-in, not and executable
* in PowerShell use `New-Item -Path <to> -ItemType SymbolicLink -Value <from>` to the same effect
