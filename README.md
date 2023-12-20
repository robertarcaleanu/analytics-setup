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
### Setting VScode

VScode is a general-purpose IDE. It supports mutliple OS such as Lunix, MacOS, Windows, and Raspberry Pi. 

Installing VScode is straightforward - go to the VScode website  [https://code.visualstudio.com/](https://code.visualstudio.com/download) and download the version depending on your OS.

On Linux, we need to folder where we downloaded the Ubuntu file and execute the following:

``` shell
sudo dpkg -i archivo.deb
```

Download the installation file and follow the instructions. Here are the default extensions settings:

``` json
"extensions": [
                
            ]
```


### Setting Python

This section focuses on setting up a Python environment.

#### Installing miniconda

Miniconda is a great tool to set local Python environments. Go to the Miniconda installer [page](https://docs.conda.io/en/latest/miniconda.html#latest-miniconda-installer-links) and download the installing package based on your operating system and Python version to install the most recent version. 

On Linux, we can install Miniconda using the following commands:

``` shell
mkdir -p ~/miniconda3
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh -O ~/miniconda3/miniconda.sh
bash ~/miniconda3/miniconda.sh -b -u -p ~/miniconda3
rm -rf ~/miniconda3/miniconda.sh

```

Once Miniconda installed you can install Python packages with `conda`:

``` shell
conda install pandas
```

Likewise, you can use `conda` to create an environment:

```
conda create -n myenv python
```

#### Common conda commands

Get a list of environments:

``` shell
conda info --envs
```

Create an environment and set the Python version:

```
conda create --name myenv python=3.9
```

Get package available versions:

```
conda search pandas
```

Activate an enviroment:

``` sheel
conda activate myenv
```

Get a list of installed packages in the environment:

``` shell
conda list
```

Deactivate the enviroment:

```shell
conda deactivate
```

### Install R and RStudio

To set in your machine R and RStudio you should start first with installing R from CRAN. Go to https://cran.r-project.org/ and select your OS.

Once R installed, you can install RStudio - go to https://posit.co website under Products tab and select [RStudio IDE](https://posit.co/downloads/) and select the version and download it.

#### Set RStudio

Next, let's set the **Global options** -> go to `Tools` and then select `Global options` and update the following:
- **General:** 
  - Workspace - select `Never` to `Save workspace to .RData on exit` option
  - History - untick the first options - `Always save history...`. This will avoid saving the session on quit
- **Code:** 
  - under the `Display` tab, tick the `Rainbow parentheses` box
- **Appearance:**  
  - select the font type and size, and editor theme
- **Pane Layout:**
  - T-L: Source
  - T-R: Console
  - B-L: Files
  - B-R: Environment  

#### RStudio main shortcuts

- Clear console - `Ctrl` +  `L`
- Clost current document - `Cmd` + `W`
- Move focus to the Source panel - `Cmd` + `1`
- Move focus to the Console panel - `Cmd` + `2`
- Move tab left - `Cmd` + `]`
- Move tab right - `Cmd` + `[`
- Move tab to first - `Cmd` + `P`
- Move tab to last - `Cmd` + `\`
- New Rmarkdown notebook - `Cmd` + `R`

## Setting Postgres

## Setting Docker
