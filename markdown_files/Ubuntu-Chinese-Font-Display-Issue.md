# Ubuntu Chinese font display issue after installl Chinese Language under `LANG=en_US`

## Description

![issues](https://i.stack.imgur.com/ANPcO.jpg)

## Solution

[Reference](https://askubuntu.com/questions/1268788/how-to-correctly-display-chinese-when-lang-en-us-utf-8)

1. Go to `/etc/fonts/conf.avail/64-language-selector-prefer.conf`
2. Change the first priority to Chinese Font Family. Like `<family>Noto Sans CJK CN</family>`
