# git cherry-pick

Here's another method just for variety.

What you should ideally do is to create new commits on top of 5 which will revert the code in commits 3 and 4.

If you have not pushed these commits, you can also do a cherry-pick. Say your repo's HEAD points at 5 and it is the branch master.

    1 -> 2 -> 3 -> 4-> 5 (HEAD, master)
    

    git co -b temp
    
    1 -> 2 -> 3 -> 4 -> 5 (HEAD, master, temp)
    

    git co master
    git reset --hard HEAD~3
    
    1 -> 2 (HEAD, master)-> 3 -> 4 -> 5 (temp)
    

    git cherry-pick temp
    
    1 -> 2 -> 3 -> 4 -> 5 (temp)
         |
         -> 5' (HEAD, master)
    

    git branch -D temp
    
    1 -> 2 -> 5' (master, HEAD)
