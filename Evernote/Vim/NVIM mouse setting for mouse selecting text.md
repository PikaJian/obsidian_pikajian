# NVIM mouse setting for mouse selecting text

    *'mouse'* *E538*
    'mouse'         string  (default "", "a" for GUI, MS-DOS and Win32)
                global
                {not in Vi}
        Enable the use of the mouse.  Only works for certain terminals
        (xterm, MS-DOS, Win32 |win32-mouse|, QNX pterm, *BSD console with
        sysmouse and Linux console with gpm).  For using the mouse in the
        GUI, see |gui-mouse|.
        The mouse can be enabled for different modes:
            n   Normal mode
            v   Visual mode
            i   Insert mode
            c   Command-line mode
            h   all previous modes when editing a help file
            a   all previous modes
            r   for |hit-enter| and |more-prompt| prompt
        Normally you would enable the mouse in all four modes with:
            :set mouse=a
        When the mouse is not enabled, the GUI will still use the mouse for
        modeless selection.  This doesn't move the text cursor.
    
        See |mouse-using|.  Also see |'clipboard'|.
    
        Note: When enabling the mouse in a terminal, copy/paste will use the
        "* register if there is access to an X-server.  The xterm handling of
        the mouse buttons can still be used by keeping the shift key pressed.
        Also see the 'clipboard' option.
