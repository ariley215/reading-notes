# Important features to look for in a text editor

- code completion
- syntax highlighting
- a variety of themes
- good selection of extensions

## Commands

**pwd** print working directory

**ls** list

**cd** change directory

**mkdir** create a directory

**touch** create new file

Whenever we refer to either a file or directory on the command line, we are in fact referring to a *path*. ie. A *path* is a means to get to a particular file or directory on the system.
  
- (~tilde) - This is a shortcut for your home directory. eg, if your home directory is /home/ryan then you could refer to the directory Documents with the path /home/ryan/Documents or ~/Documents

- (.dot) - This is a reference to your current directory. eg in the example above we referred to Documents on line 4 with a relative path. It could also be written as ./Documents (Normally this extra bit is not equired but in later sections we will see where it comes in handy)

- (..dotdot)- This is a reference to the parent directory. You can use this several times in a path to keep going up the hierarchy. eg if you were in the path /home/ryan you could run the command ls ../../ and this would do a listing of the root directory.

Arguments are extra instructions given to a command
    -  ( /etc ). When we do this it tells ls not to list our current directory but instead to list that directories contents.

cd project

    1. move to projects directory

mkdir new-project

    2. creates new directory titled new-project

touch new-project/newfile.md

    3. creates a new file in the new-project directory
cd ..

    4. This moves us to the directory that is back one level

ls projects/new-project

    5. this will list everything in the new-project directory

cp newfile new-projects --syntax: cp [options] source destination

    6. this will create a copy of newfile and put in in the new-projects directory 

    - When we use cp the destination can be a path to either a file or directory. If it is to a file then it will create a copy of the source but name the copy the filename specified in destination. If we provide a directory as the destination then it will copy the file into that directory and the copy will have the same name as the source.

mv newfile new-projects --syntax mv [options] source destination

    7. this will move newfile to the new-projects directory

    - can also rename the file or directory being moved

rm newfile

    8. deletes newfile

    -  -r option which allows for the removal of all directories and all files and directories inside that directory.

## Command Line

Within a terminal you have what is known as a *shell*. 

- This is a part of the operating system that defines how the terminal will behave and looks after running (or executing) commands for you. 

Long Listing contains:

- First character indicates whether it is a normal file ( - ) or directory ( d )
- Next 9 characters are permissions for the file or directory (we'll learn more about them in section 6).
- The next field is the number of blocks (don't worry too much about this).
- The next field is the owner of the file or directory (ryan in this case).
- The next field is the group the file or directory belongs to (users in this case).
- Following this is the file size.
- Next up is the file modification time.
- Finally we have the actual name of the file or directory.

Ex: drwxr-xr-x 18 ryan users 4096 Feb 17 09:12 Documents

Command line argument ( /etc )

- Tells ls not to list our current directory but instead to list that directories contents.

### Shortcuts

1. When you enter commands, they are actually stored in a history. You can traverse this history using the up and down arrow keys.
2. If you run the command cd without any arguments then it will always take you back to your home directory.
3. Tab Completion

- Tab key on your keyboard at any time which will invoke an auto complete action. 

- If nothing happens, there are several possibilities.

- Hit Tab again it will show you those possibilities.

4. Anything inside quotes is considered a single item.

[Command Line Cheat Sheet](https://ryanstutorials.net/linuxtutorial/cheatsheet.php)

### Sources:

[Command Line](https://ryanstutorials.net/linuxtutorial/commandline.php)
