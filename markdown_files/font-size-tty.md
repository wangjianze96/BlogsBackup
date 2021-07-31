# Change font or font size for TTY

Here are steps to set font and font size for TTY in GUI mode:

1. run `console-setup`.
```
sudo dpkg-reconfigure console-setup
```

2. choose default `UTF-8`.

3. Choose `Combined - Latin, ...`.

4. Select font: `Terminus` or `TerminusBold`.

5. Select Size: `12x24` or whatever you like.

6. Make it effect: reboot or go to a TTY and run `setupcon`.
