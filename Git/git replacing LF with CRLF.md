# git replacing LF with CRLF

These messages are due to incorrect default value of `core.autocrlf` on Windows.

The concept of `autocrlf` is to handle line endings conversions transparently. And it does!

Bad news: value needs to be configured manually.
Good news: it should only be done ONE time per git installation (per project setting is also possible).

**How `autocrlf` works**:

    core.autocrlf=true:    core.autocrlf=input:      core.autocrlf=false:
    
           repo                     repo                    repo
        /        \               /        \              /        \
    crlf->lf    lf->crlf     crlf->lf       \          /            \      
     /              \        /                \      /                \
    

Reminder: `crlf` = win-style end-of-line marker, `lf` = unix-style.

Note that `cr` (mac-style) in not affected for any of three options above.

**When does this warning show up (under Windows)**

    - `autocrlf` = `true` if you have unix-style `lf` in one of your files (= RARELY),
    - `autocrlf` = `input` if you have win-style `crlf` in one of your files (= almost ALWAYS),
    - `autocrlf` = `false` - NEVER!

**What does this warning mean**

The warning "_LF will be replaced by CRLF_" says that you (having `autocrlf`\=`true`) will lose your unix-style LF after commit-checkout cycle (it will be replaced by windows-style CRLF). Git doesn't expect you to use unix-style LF under windows.

The warning "_CRLF will be replaced by LF_" says that you (having `autocrlf`\=`input`) will lose your windows-style CRLF after a commit-checkout cycle (it will be replaced by unix-style LF). Don't use `input` under windows.

**Yet another way to show how `autocrlf` works**

    1) true:             x -> LF -> CRLF
    2) input:            x -> LF -> LF
    3) false:            x -> x -> x
    

where _x_ is either CRLF (windows-style) or LF (unix-style) and arrows stand for

    file to commit -> repository -> checked out file
    

**How to fix**

Default value for `core.autocrlf` is selected during git installation and stored in system-wide gitconfig (`%ProgramFiles(x86)%\git\etc\gitconfig`). Also there're (cascading in the following order):

   - "global" (per-user) gitconfig located at `~/.gitconfig`, yet another
   - "global" (per-user) gitconfig at `$XDG_CONFIG_HOME/git/config` or `$HOME/.config/git/config` and
   - "local" (per-repo) gitconfig at `.git/config` in the working dir.

So, write `git config core.autocrlf` in the working dir to check the currently used value and

   - add `autocrlf=false` to system-wide gitconfig             # per-system solution
   - `git config --global core.autocrlf false`        # per-user solution
   - `git config --local core.autocrlf false`          # per-project solution

**Moral** (for Windows):
    - use `core.autocrlf` = `true` if you plan to use this project under Unix as well (and unwilling to configure your editor/IDE to use unix line endings),
    - use `core.autocrlf` = `false` if you plan to use this project under Windows only,
    - _never_ use `core.autocrlf` = `input` unless you have a good reason to (_eg_ if you're using unix utilities under windows or if you run into makefiles issues),

P.S. **What to choose when installing git for Windows?**
If you're not going to use any of your projects under Unix, _don't_ agree with the default first option. Choose the third one (**Checkout as-is, commit as-is**). You won't see this message. Ever.

P.P.S. My personal preference is configuring the **editor/IDE** to use Unix-style endings, and setting `core.autocrlf` to `false`.
