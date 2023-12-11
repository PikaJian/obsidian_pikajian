# Changing default width of blocks in beamer

You could define your own block environment with an optional parameter for its width and provide a default value such as `.9\textwidth`.

Here's an example:

    \documentclass{beamer}
    \usetheme{Warsaw}
    \usepackage[english]{babel}
    
    \newenvironment<>{varblock}[2][.9\textwidth]{%
      \setlength{\textwidth}{#1}
      \begin{actionenv}#3%
        \def\insertblocktitle{#2}%
        \par%
        \usebeamertemplate{block begin}}
      {\par%
        \usebeamertemplate{block end}%
      \end{actionenv}}
    
    \begin{document}
    
    \begin{frame}
    \begin{block}{Standard}
      Normal block
    \end{block}
    \begin{varblock}[4cm]{New block}
      Variable width, here 4cm
    \end{varblock}
    \begin{varblock}{New block}
      If no width was given, .9\textbackslash textwidth will be used
    \end{varblock}
    \end{frame}
    
    \end{document}

![[P5s0F.png]]
