## Installation


Please refer to the [guide](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git).

## Configure 
Please complete all the steps below by `git bash` in windows or `CLI` in Linux

## Your Identity
The first thing you should do when you install Git is to set your user name and email address. This is important because every Git commit uses this information, and it’s immutably baked into the commits you start creating:

```bash
$ git config --global user.name "John Doe"
$ git config --global user.email johndoe@example.com
```

Again, you need to do this only once if you pass the --global option, because then Git will always use that information for anything you do on that system. If you want to override this with a different name or email address for specific projects, you can run the command without the --global option when you’re in that project.


## Your Editor
Now that your identity is set up, you can configure the default text editor that will be used when Git needs you to type in a message. If not configured, Git uses your system’s default editor.

If you want to use a different text editor, such as VIM, you can do the following:
```
$ git config --global core.editor vim
```

While on a Windows system, if you want to use a different text editor, such as Notepad++, you can do the following:

On a x86 system
```
$ git config --global core.editor "'C:/Program Files/Notepad++/notepad++.exe' -multiInst -nosession"
```
On a x64 system
```
$ git config --global core.editor "'C:/Program Files (x86)/Notepad++/notepad++.exe' -multiInst -nosession"
```

## Checking Your Settings
If you want to check your settings, you can use the git config --list command to list all the settings Git can find at that point:
```
$ git config --list
user.name=John Doe
user.email=johndoe@example.com
color.status=auto
color.branch=auto
color.interactive=auto
color.diff=auto
...
```
You may see keys more than once, because Git reads the same key from different files (`/etc/gitconfig` and `~/.gitconfig`, for example). In this case, Git uses the last value for each unique key it sees.

You can also check what Git thinks a specific key’s value is by typing `git config <key>`:
```
$ git config user.name
John Doe
```

## Using `ssh`
### Generating a new SSH key on windows
```
cd 
mkdir .ssh
cd .ssh
ssh-keygen -C 'YOUR_EMAIL@EMAIL.COM'
```
Don't set 'passphrase' if you don't want to type password everytime you use it. If you want to know more. [Do you need a passphrase](http://superuser.com/questions/261361/do-i-need-to-have-a-passphrase-for-my-ssh-rsa-key).

Copy and paste your **complete** public key to our [git server](http://ict.eri.ntu.edu.sg:8002/profile/keys).

## Getting Help
If you ever need help while using Git, there are three ways to get the manual page (manpage) help for any of the Git commands:
```
$ git help <verb>
$ git <verb> --help
$ man git-<verb>
```
For example, you can get the manpage help for the config command by running

```
$ git help config
```
These commands are nice because you can access them anywhere, even offline. If the manpages and this book aren’t enough and you need in-person help, you can try the `#git` or `#github` channel on the Freenode IRC server (`irc.freenode.net`). These channels are regularly filled with hundreds of people who are all very knowledgeable about Git and are often willing to help.