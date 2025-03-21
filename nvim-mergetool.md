# Documentation for NeoVim Mergetool #

## Getting and putting diff changes ##

In order to get or put changes in a buffer from other buffers, we can user the
following commands:

```vim
:diffget OTHER_FILE
:diffput OTHER_FILE
```

This will get or put the changes from the `OTHER_FILE` to the current buffer
from the current diff. If we want to get multiple diff changes, we can select
them and apply the command to it.
