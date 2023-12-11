# zsh history

You've probably got `INC_APPEND_HISTORY` set.

The `INC_APPEND_HISTORY` option, from `man zshoptions`:

> This options works like APPEND\_HISTORY except that new history lines are added to the $HISTFILE incrementally (as soon as they are entered), rather than waiting until the shell exits.

The option that you want is `APPEND_HISTORY`:

> APPEND\_HISTORY If this is set, zsh sessions will append their history list to the history file, rather than replace it. Thus, multiple parallel zsh sessions will all have the new entries from their history lists added to the history file, in the order that they exit. The file will still be periodically re-written to trim it when the number of lines grows 20% beyond the value specified by $SAVEHIST (see also the HIST SAVE BY COPY option).

You can read about these options in the `man zshoptions`, `man zshall` or online [here](http://linux.die.net/man/1/zshoptions).

To set them, in your `~/.zshrc` or similar, you should have:

    setopt APPEND_HISTORY

Be aware that, if you're using [oh-my-zsh](https://github.com/robbyrussell/oh-my-zsh) by default, I believe, `INC_APPEND_HISTORY` is used. I'm not 100% sure which way around things get loaded, but if the `oh-my-zsh` option overrides the one you've set in `~/.zshrc`, you can fiddle with it in `~/.oh-my-zsh/lib/history.zsh`
