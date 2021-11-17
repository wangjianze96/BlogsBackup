# Arch Linux locally signing key error

This error normally will come up when user try to locally signing a new gpg key from a third party.

Can be solved by remove the gunpg folder and reinitializing it.

```
$ sudo rm -rf /etc/pacman.d/gunpg/
$ sudo pacman-key --init
$ sudo pacman-key --populate archlinux
```

Then try again, the problem should be gone.

[reference1](https://archlinux.org/news/gnupg-21-and-the-pacman-keyring/)

[reference2](https://bbs.archlinux.org/viewtopic.php?id=191476)
