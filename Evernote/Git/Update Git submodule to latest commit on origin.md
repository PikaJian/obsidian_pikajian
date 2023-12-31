# Update Git submodule to latest commit on origin

The [`git submodule update`](http://git-scm.com/docs/git-submodule) command actually tells Git that you want your submodules to each check out the commit already specified in the index of the superproject. If you want to _update_ your submodules to the latest commit available from their remote, you will need to do this directly in the submodules.

So in summary:

    # get the submodule initially
    git submodule add ssh://bla submodule_dir
    git submodule init
    
    # time passes, submodule upstream is updated
    # and you now want to update
    
    # change to the submodule directory
    cd submodule_dir
    
    # checkout desired branch
    git checkout master
    
    # update
    git pull
    
    # get back to your project root
    cd ..
    
    # now the submodules are in the state you want, so
    git commit -am "Pulled down update to submodule_dir"
    

Or, if you're a busy person:

    git submodule foreach git pull origin master
