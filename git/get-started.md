## Installation


Please refer to the [guide](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git).

## Configure 

Your Identity
The first thing you should do when you install Git is to set your user name and email address. This is important because every Git commit uses this information, and it’s immutably baked into the commits you start creating:

```bash
$ git config --global user.name "John Doe"
$ git config --global user.email johndoe@example.com
```
Again, you need to do this only once if you pass the --global option, because then Git will always use that information for anything you do on that system. If you want to override this with a different name or email address for specific projects, you can run the command without the --global option when you’re in that project.

## Initiate a git project:

```
mkdir PROJECT_NAME
cd PROJECT_NAME
git init
```

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