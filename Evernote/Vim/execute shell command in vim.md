# execute shell command in vim

Vim is a powerful editing tool, but there are some things it just can't do. However, Vim lets you access shell commands and utilities without leaving Vim, and that lets you perform some amazing tricks.

If you run `:shell` or just `:sh` while you're in the editor, Vim (or Gvim, if you're partial to Vim's GUI) will place you in an interactive shell. You can run whatever commands you want, and resume your Vim session by exiting the shell.

As most other \*nix applications, you can also pause Vim with `Ctrl-z`, which will drop you back to the shell. When you're finished, you can resume Vim with `fg`. (This is a feature of the shell, not a Vim feature.)

Have you ever started editing a file, made a bunch of changes, and then typed `:w` to write your changes, only to find that the file is read-only? You can deal with that in a couple of ways, but one of the easiest things to do is to invoke a shell from within Vim and change the file's permissions before you save it again.

**Bang!**

Vim also allows you to execute a command directly from the editor, without needing to drop to a shell, by using [bang](http://www.eps.mcgill.ca/jargon/jargon.html#bang) (`!`) followed by the command to be run. For instance, if you're editing a file in Vim and want to find out how many words are in the file, run

`:! wc %`

This tells Vim to run the file (`%`) through the `wc` utility and report the results. Vim will display the command output at the bottom of the editing window until you press `Enter`. Note that this command runs the file through the command, and not the contents of the buffer -- so if you haven't saved recently, it won't report your most recent word count.

Bang works best with non-interactive commands. You wouldn't want to run `top` or another interactive command using `:! _command_` , but you could drop to a shell and run such a command with `:sh` or `Ctrl-z`.

The bang command can be useful if you're using Vim for programming. If you're writing a PHP script, for example, you could use PHP's syntax check option (`-l`) to see if your script has any syntax errors:

`:! php5 -l %`

If you're working on a script or project, you might want to check it regularly, and with minimal typing. You can scroll through Vim's command history by using the up arrow, but if you just want to rerun the last external command, you can use `:!!` instead.

**Reading command output**

Most Vim users are already familiar with using the read command, which inserts text from a specified file into the current buffer, like so:

`:r _textfile_`

This is handy, but many users aren't aware that you can also read in the output of shell applications. For example, if you wanted to include a list of files from a specific directory, you could include them using this read command:

`:r ! ls -1 /home/user/directory`

This tells Vim to execute the command `ls -1 /home/user/directory` and then redirect the output of that command into the buffer. You could also use this feature to read the text from a Web page into the file that you're editing, using a text-mode browser such as w3m. It's pretty simple to grab a page using w3m and dump it right into your editing session without leaving Vim:

`:r ! w3m http://en.wikipedia.org/wiki/Vi -dump`

The `-dump` option tells w3m to simply spit out the Web page as plain text and exit.

It's also possible to execute multiple commands to process your text before reading it into Vim. A simple example would be to list the directory contents, then pipe the output to the sort command to sort the filenames in reverse order, before inserting the text into the current buffer:

`:r ! ls -1 /home/user/directory | sort -r`

The read command can also come in handy if you're prepping an incident report from server logs. Let's say you wanted to include all of the errors in an Apache log that include a specific string. You could grep the log and insert the input into your Vim session:

`:r ! grep _string_ /var/log/apache2/site-error.log`

Vim makes it easy to redirect the output of most standard \*nix utilities into a file, and to pipe text from the file it's editing into standard \*nix utilities.

**Filtering text through external filters**

While in Vim, you can select a range of text and run that text through an external command. In visual mode, just highlight the text you want to work with, then run `:! _commandname_` . The highlighted text will be replaced by the output of the command. Let's say you wanted to use weak encryption on part of a file by running it through the rot13 utility. Use `v`, `V`, or `Ctrl-v` to enter the visual mode of your choice and select the text you want to encrypt. Then run `:! rot13` to replace the selected text with rot13-encoded text. This might be handy when composing an email containing a spoiler about the latest episode of _Battlestar Galactica_ for sending to a mailing list. It's trivial for recipients to decode rot13-encoded text, but the folks who don't like spoilers won't have any surprises ruined by skimming over the text.

You can also specify a range of lines to process, rather than selecting lines in visual mode. For instance, to filter lines 20 through 25 of a file through rot13:

`:20,25 ! rot13`

Vim will run the text through rot13 and insert it in place of the existing text. If you accidentally overwrite some text in Vim that you didn't mean to, don't worry -- Vim's undo command (`u`) allows you to restore the original text.

**Using a different shell**

By default, Vim should use your default shell. If you want to make sure that Vim's using your default shell, you can check the value of the shell option by running this query:

`:set shell ?`

Vim will display `shell=/bin/bash`, or whatever the shell is set to. Note that this works with other options as well, so you can use `:set _option_ ?` to check the value of any option within Vim.

If you'd like to change the value of the shell temporarily, run `:set shell=_/path/to/shell_` with the correct path to your chosen shell. For instance, on Ubuntu, if you want to use the Korn shell, you can run `:set shell=/usr/bin/ksh` -- assuming you have the Korn shell installed.

To make the change of shells permanent, open your ~/.vimrc and add this line:

`set shell=_/path/to/shell_`

The next time you start a Vim session, the new shell option will be set.

Get in the habit of using Vim's `:shell` command and the `:!` utility, and you'll find that you can be even more productive while using Vim than you might have thought possible.
