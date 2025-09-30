More fun with C standards. Using POSIX, ISO C and MISRA, write a function for a thread that executes an infinite loop (a fairly common pattern, especially in embedded systems): 

1. Can't use return NULL after the infinite loop, because that's dead code. 
2. Can't mark the function as _Noreturn, because that cannot be done for a function not returning void. 
3. Can't pass void (*)(void *) to pthread_create() without a cast. 
4. Can't cast the function to (void *) due to MISRA required rules. 
5. Can't cast the function to void *(*)(void *), because GCC emits an error. 

I ended up adding a pthread_create_detached() function to the QNX C library (it's good to be king!), which takes void (*)(void *), as well as sets the PTHREAD_CREATE_DETACHED flag automatically. I have a great sense of accomplishment, and feel that the code is now much safer and more secure...