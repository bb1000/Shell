<script type="text/javascript"
  src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML">
</script>
# Linux

## 

BB1000 Programming in Python

KTH

---

layout: false

## Learning objectives

To be able to work with Linux command-line interface (CLI).

+ Bash basics

+ Files (move/copy/remove)

+ Permissions (read/write/execute)

+ Configure your bash with .bashrc

+ Use a text editor

+ Retrieve information from files

---

## Shell basics

+ A shell is...

  - a user interface

  - the outermost layer around the operating system kernel

+ This lecture focuses on...

  - text shell (command-line interface)

  - <a href="https://en.wikipedia.org/wiki/Bash_(Unix_shell)">bash</a> in particular

+ How to start a bash shell

  - On Linux/Mac: Open a terminal window.

  - On Windows: Use [WSL](https://docs.microsoft.com/en-us/windows/wsl/install-win10).

---

## First essentials

To show the directory (folder) your are in: `pwd`

List all the files (and directories) in the current directory: `ls`

Make a directory: `mkdir my_dir`

```
$ pwd
/home/kronop

$ ls
examples.desktop

$ mkdir my_dir

$ ls
examples.desktop  my_dir
```

---

layout: false

## First essentials

To show in which directory the cursor is located: `pwd`

List all the files (and directories) in the current directory: `ls`

Make a directory: `mkdir my_dir`

```
$ pwd
/home/kronop

$ ls
examples.desktop

$ mkdir my_dir

$ ls
examples.desktop my_dir
```

Useful flag for mkdir:

```
mkdir -p my_dir

mkdir -p first_dir/second_dir/third_dir
```

---

layout: false

## First essentials (continued)

Go into the directory: `cd my_dir`

Create an empty file: `touch my_file`

Go out of the directory: `cd ..` 

```
$ cd my_dir

$ pwd
/home/kronop/my_dir

$ touch my_file

$ ls
my_file

$ cd ..

$ pwd
/home/kronop

$ ls
examples.desktop  my_dir
```

---

## Move files

The command `mv` can be used to 

  + rename a file

  + move one or more files to another directory

```
$ cd my_dir

$ ls
my_file

$ mv my_file my_file_new

$ ls
my_file_new

$ mv my_file_new ../

$ cd ..
```

---

## Move files (continued)

The command `mv` can be used to 

  + rename a file

  + move one or more files to another directory

```
$ pwd
/home/kronop

$ ls
examples.desktop my_dir my_file_new

$ mv my_file_new my_dir/my_file

$ ls my_dir 
my_file
```

---

## Copy files

Files are copied using `cp` - note that the user always has to specify the destination.

```
$ cd my_dir

$ cp my_file copy_of_my_file

$ ls
my_file copy_of_my_file

$ cp copy_of_my_file ../

$ cd ..

$ pwd
/home/kronop

$ ls
examples.desktop  my_dir copy_of_my_file

$ ls my_dir 
my_file copy_of_my_file
```

---

## Remove files

Files and directories can be removed by command `rm`.

```
$ pwd
/home/kronop

$ rm copy_of_my_file my_dir/copy_of_my_file

$ ls
examples.desktop  my_dir

$ ls my_dir
my_file
```

---

## Commands so far...

+ `pwd`: print current working directory

+ `ls`: list files and directories

+ `mkdir`: create directory

+ `cd`: change directory 

+ `touch`: create empty file; update file/directory

+ `mv`: rename/move file and/or directory

+ `cp`: copy file and/or directory

+ `rm`: remove file and/or directory

Quick reference guide:

https://files.fosswire.com/2007/08/fwunixref.pdf

---

## Relative vs. absolute paths

```
$ pwd
/home/kronop

$ ls my_dir
my_file

$ ls /home/kronop/my_dir
my_file

$ cd my_dir

$ ls
my_file

$ cd /home/kronop/my_dir

$ ls
my_file

$ cd
```

---

## Hidden files

A file is hidden if its name starts with a ".". Hidden files are only listed by
`ls` with the `-a` flag.

With `ls -alh` all files in the directory are given - the normal files as well
as the hidden ones. The files are listed with their permissions, the name of
their owner and the group of the owner. 

```
$ ls -alh
drwxr-xr-x  3 kronop sknippen 4,0K nov 17  2016 .
drwxr-xr-x 31 root   root     4,0K feb  4 20:54 ..
-rw-------  1 kronop sknippen   59 nov 17  2016 .bash_history
-rw-r--r--  1 kronop sknippen  220 nov 17  2016 .bash_logout
-rw-r--r--  1 kronop sknippen 3,6K nov 17  2016 .bashrc
drwx------  2 kronop sknippen 4,0K nov 17  2016 .cache
-rw-r--r--  1 kronop sknippen 8,8K nov 17  2016 examples.desktop
drwxr-xr-x  2 kronop sknippen 4,0K feb 11 21:39 my_dir
```

User kronop is a member of the group around user sknippen. The date of creation
of the files is given (17 nov 2016), as well as the extend of the files (in
Bytes). The response above can be compared with

```
$ ls
examples.desktop my_dir
```

---

## Permissions

The permissions of the files and directories are given in the first column.

+ The first symbol is `d` for directory and `-` for file.

+ The next three symbols denote the permission for the owner of the file: `rwx`
  points at reading, writing and executing.

+ The following three symbols denote the permission for members of the group
  sknippen, of which the user kronop is a member.

+ The last three symbols denote the permission for all other users.

```
$ ls -alh
drwxr-xr-x  3 kronop sknippen 4,0K nov 17  2016 .
drwxr-xr-x 31 root   root     4,0K feb  4 20:54 ..
-rw-------  1 kronop sknippen   59 nov 17  2016 .bash_history
-rw-r--r--  1 kronop sknippen  220 nov 17  2016 .bash_logout
-rw-r--r--  1 kronop sknippen 3,6K nov 17  2016 .bashrc
drwx------  2 kronop sknippen 4,0K nov 17  2016 .cache
-rw-r--r--  1 kronop sknippen 8,8K nov 17  2016 examples.desktop
drwxr-xr-x  2 kronop sknippen 4,0K feb 11 21:39 my_dir
```

---

## Permissions (continued)

In many cases, the group members are allowed to read the files (`r`) and
execute them (`x`).

A normal user most likely would not allow other people to change his/her files,
so writing permision (`w`) is advised to be disabled for groupmembers and
certainly for other people.   

Programs and scripts are usually made executable (`x`). 

```
$ touch my_empty_program

$ ls -alh my_empty_program
-rw-r--r-- 1 kronop sknippen 0 feb 11 21:54 my_empty_program
```

---

## Change permissions

The file `my_empty_program` is readable and writable for kronop, and it
is just readable for the members of the group sknippen and for the rest of the
users. It is not yet executable, therefore `chmod` is used.

`chmod u+x` renders the file for the user kronop to an executable file. `chmod
g+x` gives also the members of the group the opportunity to run the file.

```
$ chmod u+x my_empty_program

$ ls -alh my_empty_program
-rwxr--r-- 1 kronop sknippen 0 feb 11 21:54 my_empty_program

$ chmod g+x my_empty_program

$ ls -alh my_empty_program
-rwxr-xr-- 1 kronop sknippen    0 feb 11 21:54 my_empty_program
```

---

## The Bash shell startup file: ~/.bashrc

In the .bashrc file, the personalized settings for the user kronop are defined.  

From a cautious point of view, important settings could be:

```
alias rm="rm -i"
alias cp="cp -i"
alias mv="mv -i"
```

Thanks to these settings, Linux will ask you whether you would really like to
remove (`rm`) or overwrite (in case of `cp` and `mv`) files.

In case you write your own programs, you have to tell linux where they are
located. Generally, a typical user like kronop has the habit to locate them in
the /home/kronop/bin folder, which has to be loaded in the .bashrc file:

```
export PATH=/home/kronop/bin:$PATH
```

---

## The Bash shell startup file: ~/.bashrc (continued)

Other useful settings:

```
alias ll="ls -l"
alias grep="grep --color"
```

When the .bashrc file has been changed, you have to reload or `source` it so
that the changes can take effect.

```
$ cd

$ pwd
/home/kronop

$ source .bashrc
```

If you are not in your home directory

```
$ source ~/.bashrc
```

---

## Text editor

`nano` is a basic text-editor.

`nano filename` opens a file named `filename`.

At the bottom of the window, the various tools are given.

  - '^X' points at 'ctrl-X' and means close (without saving).

  - To save, 'ctrl-O' is used - afterwhich the texteditors asks a confirmation
    of the filename.

  - If you don't want to save the information under another filename, press on
    enter.

  - However, when you do want to change filename, `nano` will ask another
    confirmation (Yes/No).  

Other commonly used text-editors are `gedit`, `vi` and `emacs`.

---

## Finding files

It is a general advice to keep all your files ordered. It might be easy at this
moment to find files back, but within three months, you should still know which
file was connected with what project and how it was created... 

However, to search files `find . -name` is used. The `.` points at the current
directory where (and down) the file is searched.

```
$ pwd
/home/kronop

$ find . -name my_file
./my_dir/my_file
```

---

## Searching information in a file

Imagine user kronop has to search the birth date of a friend Erik in a
file.

```
$ cat birthdates
Emma 1981-04
Erik 1972-12
Mohammed 1988-02
Helen 1945-09
(...)
```

`cat` prints the content of a file to screen - but it cannot be changed.

In stead of browsing through the file, the linux function `grep` can be used.

```
$ grep Erik birthdates
Erik 1972-12
```

---

## What is the structure among my files?

At each instance in the hierarchy of the data, command `tree` can show the
structure of the directories and the files at that position and below.

```
$ pwd
/home/kronop

$ tree
.
├── examples.desktop
└── my_dir
    ├── birthdates
    ├── my_empty_program
    └── my_file
```

---

## When you don't know...

Linux has a built-in help function, which is especially useful when you do not
know which optional parameters can be used... Simply write `man` followed by
the command you would like to have more information about. `man ls` will give
various pages of information about the `ls` command...

```
$ man ls

LS(1)                            User Commands                           LS(1)

NAME
       ls - list directory contents

SYNOPSIS
       ls [OPTION]... [FILE]...

DESCRIPTION
       List  information  about  the FILEs (the current directory by default).
       (...)
		     
```

<!--
% is modulo, ** is power
-->

