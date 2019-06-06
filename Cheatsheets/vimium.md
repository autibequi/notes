# vimium - basic edition

Vimium -default- bindings are too complex (for me) and makes me do a lot of mistakes. Here is a `just enought` Edition:

```shell
# Fresh Start
unmapAll

# Movement
map h scrollLeft
map j scrollDown
map k scrollUp
map l scrollRight

map d scrollPageDown
map u scrollPageUp

map gg scrollToTop
map G scrollToBottom

# Open Link
map f LinkHints.activateMode
map F LinkHints.activateModeToOpenInNewTab

# History
map H goBack
map L goFoward

# Tabs
map t createTab
map J previousTab
map K nextTab
map p togglePinTab

map ,, moveTabLeft
map .. moveTabRight

# Misc
map yy copyCurrentUrl
map ? showHelp
```
