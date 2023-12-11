# tmux and fzf

1. in tmux.conf set -g @plugin 'christoomey/vim-tmux-navigator'
2. start tmux and prefix+I to install plugin
3. modify .tmux/plugins/vim-tmux-navigator/vim-tmux-navigator.tmux below 

is\_vim="ps -o state= -o comm= -t '#{pane\_tty}' \\
    | grep -iqE '^\[^TXZ \]+ +(\\\\S+\\\\/)?g?(view|n?vim?x?|fzf)(diff)?
"
