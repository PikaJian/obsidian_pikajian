# redirect console

no stdout :
xxxx.app > xxx.log 2>&1

both stdout and file :
xxxx.app 2>&1 | tee xxx.log
