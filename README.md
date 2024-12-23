# Learning Git
Learning git commands

## Linux Commands

- cd - move into or out of a directory(change directory)
- mkdir - creates a new directory(short for make directory)
- pwd - print out the current directory(short for print working directory)
- echo - display a text or string in the terminal
- command > file - the output of the command will be written to the file and if the file exists the contents of file will be overwritten 
- command >> file - the output of the command will be written to the file and if the file exists the output will be appended to the file
- nano file - opens a text editor in the command line and allows editing of files.

## Git Commands
- `git status` - provides info on the current state of the orking directory and staging area
- `git add file-name` - stages a file or changes to the file
- `git add .` - stages all untracked changes
- `git commit` - commit the staged changes to the git repository
- `git commit -m "commit message"` - allows the user to add a message to the commit
- `git commit -am "commit message"` - combines the staging of all modified files and committing the staged changes
- `git push origin main` - pushes the changes in your current branch (here it is main) to the remote repository's main branch
- `git push -u origin main` - sets the upstream tracking information for your local branch 
