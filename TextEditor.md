## Important features to look for in a text editor

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
  
+  ~ (tilde) - This is a shortcut for your home directory. eg, if your home directory is /home/ryan then you could refer to the directory Documents with the path /home/ryan/Documents or ~/Documents

+ . (dot) - This is a reference to your current directory. eg in the example above we referred to Documents on line 4 with a relative path. It could also be written as ./Documents (Normally this extra bit is not required but in later sections we will see where it comes in handy)

+ .. (dotdot)- This is a reference to the parent directory. You can use this several times in a path to keep going up the hierarchy. eg if you were in the path /home/ryan you could run the command ls ../../ and this would do a listing of the root directory.

Arguments are extra instructions given to a command
    -  ( /etc ). When we do this it tells ls not to list our current directory but instead to list that directories contents.

cd projects 
    
    1. move to projects directory

mkdir new-project
   
    2. creates new directory titled new-project

touch new-project/newfile.md
   
    3. creates a new file in the new-project directory

cd ..
    
    4. This moves us to the directory that is back one level

ls projects/new-project
   
    5. this will list everything in the new-project directory

