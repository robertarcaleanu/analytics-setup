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
### Set Git global options

Git enables setting both local and global options. The global options will be used as default settings any time triggering a new repository with the `git init` command. You can override the global settings on specific repo by using local settings. Below, we will define the following global settings:

- Git user name
- Git user email
- Default branch name
- Global git ignore file
- Default editor (for merging comments)

### Set git user name and email

Setting global user name and email by using the `config --global` command:
``` shell
git config --global user.name "USER_NAME"
git config --global user.email "YOUR_EAMIL@example.com"
```
### Set default branch name

Next, let's set the default branch name as `main` using the `init.defaultBranch` argument:

``` shell
git config --global init.defaultBranch main
```

### Set default editor

Git enables you to set the default shell code editor to create and edit your commit messages with the `core.editor` argument. Git supports the main command line editors such as `vim`, `emacs`, `nano`, etc. I set main as `vim`:

``` shell
git config --global core.editor "vim"
```

### Review and modify global config settins

By default, all the global settins saved to the `config` file under the `.ssh` folder. You can review the saved settings, modify and add new ones manually by editing the `config` file:


``` shell
vim ~/.gitconfig
```


## Set SSH with Github

Setting `SSH` key is required to sync your local git repositories with the `origin`. By default, when creating the SSH keys it writes the files under the `.ssh` folder, if exists, otherwise it writes it down under the root folder. It is more "clean" to have it under the `.ssh` folder, therefore, my settings below assume this folder exists. 

Let's start by creating the `.ssh` folder:

``` shell
mkdir ~/.ssh
```
We need to change the directory to the newly created folder:

``` shell
cd ssh
```

The `ssh-keyget` command creates the SSH keys files:

To set SSH key on your local machine you need to use `ssh-keyget`:

``` shell
ssh-keygen -t ed25519 -C "YOUR_EAMIL@example.com"
```
**Note:** The `-t` argument defines the algorithm type for the authentication key, in this case I used `ed25519` and the `-C` argument enables adding comment,in this case the user name email for reference.

After runngint the `ssh-keygen` command, it will prompt for setting file name and password (optional). By default it will save it under the root folder. 

**Note:** this process will generate two files:
- `your_ssh_key` is the private key, you should not expose it
- `your_ssh_key.pub` is the public key which will be used to to set the SSH on Github

The next step is to register the key on your Github account. On your account main page go to the `Settings` menu and select on the main menu `SSH and GPG keys` (purple rectangle 👇🏼) and click on the `New SSH key` (yellow rectangle 👇🏼):

<img width="1054" alt="Screenshot_ssh1" src="images/git_ssh1.png">


Next, set the key name under the title text box (purple rectangle 👇🏼), and paste your public key to the `key` box (turquoise rectangle 👇🏼):

<img width="1054" alt="Screenshot_ssh2" src="images/git_ssh2.png">

**Note:** I set the machine nickname (e.g., MacBook Pro 2017, Mac Pro, etc.) as the key title to easily identify the relevant key in the future.

Next step is to update the `config` file on the `~/.ssh` folder. You can edit the `config` file with `vim`:

``` shell
vim ~/.ssh/config 
```

and add somewhere on the file the following code:

``` shell
Host *
  AddKeysToAgent yes
  UseKeychain yes
  IdentityFile ~/.ssh/your_ssh_key
```
Where `your_ssh_key` is the private key file name


Last, run the following to load the key:
```
ssh-add --apple-use-keychain ~/.ssh/your_ssh_key
```

