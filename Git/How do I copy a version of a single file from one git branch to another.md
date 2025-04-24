# How do I copy a version of a single file from one git branch to another?

Run this from the branch where you want the file to end up:

    git checkout otherbranch myfile.txt
    

General formulas:

    git checkout <commit_hash> <relative_path_to_file_or_dir>
    git checkout <remote_name>/<branch_name> <file_or_dir>
    

Some notes (from comments):

* Using the commit hash you can pull files from any commit

* This works for files and directories
* overwrites the file `myfile.txt` and `mydir`
* Wildcards don't work
* but relative paths do

an alternative:

    git show commit_id:path/to/file > path/to/file
