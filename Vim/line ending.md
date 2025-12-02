Change the line endings in the view:

```
:e ++ff=dos
:e ++ff=mac
:e ++ff=unix
```

This can also be used as saving operation (:w alone will not save using the line endings you see on screen):

```
:w ++ff=dos
:w ++ff=mac
:w ++ff=unix
```