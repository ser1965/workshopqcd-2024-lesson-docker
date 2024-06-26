---
title: Setup
---

## Working environment

::::::::::::::::::::::::::::::::::::::: discussion

### Note

We expect participants to work in a Unix environment on their laptop.

If you are unfamiliar with the Unix shell, you can work through the exercises in [The Unix Shell](https://swcarpentry.github.io/shell-novice/) tutorial by Software Carpentry. 

Windows users: the Unix tutorial gives `git bash` as an option. However, for all work during the hands-on session:

- do **not** use `git bash`
- do **not** use `Power shell` (apart from installing WSL2)
- activate WSL2 and **use the Ubuntu terminal** that comes with it!

:::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::: spoiler

### Windows

Activate [WSL2](https://learn.microsoft.com/en-us/windows/wsl/install#install-wsl-command) and use the Ubuntu terminal. 

Note that if the Ubuntu terminal does not open properly, you might need to enable Virtualization in the BIOS menu. 

You might also need to set a parameter in Command prompt (run it as admin):

```
bcdedit /set {current} hypervisorlaunchtype auto
```

and restart the computer.

Note that if you use virtual images e.g. VirtualBox on the same computer, you need to set that parameter to `off` and restart. So no WSL2 and VirtualBox in the same session.

::::::::::::::::::::::::

:::::::::::::::: spoiler

### MacOS

Use Terminal.app

::::::::::::::::::::::::


:::::::::::::::: spoiler

### Linux

Use Terminal

::::::::::::::::::::::::
