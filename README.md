This repository contains the steps that should be followed to configure my default Data Sciente/Analytics setup.

This document covers the following:
- Setting Git and SSH
- Setting VSCode
- Setting Python
- Install R and RStudio
- Install Docker
- Setting Postgres

_This document is inspired by [Rami Krispin's](www.github.com) tutorial._

## Setting Git and SSH

This section focuses on the core git settings, such as global definitions and setting SSH with your Github account.

All the settings in the sections are done through the command line (unless mentioned otherwise).

Let's start by checking the `git` version running the following:

``` shell
git --version
```

If this is a new computer or you did not set it before, it should prompt a window and ask you if you want to install the `command line developer tools`. In Linux, we can install it running the following:
```shell
sudo apt install git
```

