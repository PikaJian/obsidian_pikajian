# Vim Fugitive commit flow

There are a few ways to accomplish this task. I will outline the most interactive method which uses `:Gstatus`.

* Open up the status window via `:Gstatus`

* Move between files via `<c-n>`/`<c-p>`
* Stage/unstage files via `-`
* Start committing via `cc` whilst in the status window
* Create commit message and save and close window. (I prefer `:x`)
* You can also use `zj` and `zk` to move between sections
* Using `-` on a section will stage/unstage all the files in that section

For more help with `:Gstatus` see `:h :Gstatus` or `g?` when in the `:Gstatus` buffer.

Can use `:Gwrite` and `:windo` to skip the `:Gstatus` window to make this a bit faster.

    :windo Gw
    :Gcommit
    

You can also skip the whole commit window by using the `-m` flag. e.g. `:Gcommit -m "A short message"`

I recommend [Vimcasts](http://vimcasts.org/) videos on Fugitive: [The Fugitive Series - a retrospective](http://vimcasts.org/blog/2011/05/the-fugitive-series/)

To learn more see:

    :h fugitive
    :h :Gstatus
    :h :Gw
    :h :Gcommit
    :h :windo
    :h :x
