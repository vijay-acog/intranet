#  Computing environment(M1) at Aganitha

Installation and customization of software on mac. It enables you to work on remote machines, develop software, and deploy software  

**For Macbook M1 Users**

[**ChangeLog	1**](#changelog)

[**Background	1**](#background)

[**Installation of software	1**](#installation-of-software)

[**Setting up keys	1**](#setting-up-keys)

[**Installing homebrew	2**](#installing-homebrew)

[Python and Python Version Manager Installation	3](#python-and-python-version-manager-installation)

[Managing virtual environments using pyenv	4](#managing-virtual-environments-using-pyenv)

[**UV Python Version Manager Installation(new)	4**](#uv-python-version-manager-installation\(new\))

[Managing projects using uv	4](#managing-projects-using-uv)

[Installing node	4](#installing-node)

[**Setting up vscode	4**](#setting-up-vscode)

[**Setting up Pycharm	6**](#setting-up-pycharm)

[**Introduction to our machines	6**](#introduction-to-our-machines)

[**Git: basics you need to know	7**](#git:-basics-you-need-to-know)

[**Installing Aganitha Tool Kit (atk)	8**](#installing-aganitha-tool-kit-\(atk\))

[Standards	8](#standards)

[**Appendix:	8**](#appendix:)

[**Setup your personal virtual machine (only if needed)	8**](#setup-your-personal-virtual-machine-\(only-if-needed\))

# ChangeLog {#changelog}

This document is updated to make it comprehensive. 

1. Added: Working on remote machines  
2. Added: Customising macbooks  
3. Added: Updated pyenv config that we add to \~/.zshrc file  
4. Added: Setting up uv package

# Background {#background}

This document is focused on M1 mac. It can be adapted to work on Intel as well. I marked it as such wherever possible. The course [https://classroom.google.com/c/MzQwMDcwMTU5NTcy](https://classroom.google.com/c/MzQwMDcwMTU5NTcy) accompanies this document. 

# Installation of software {#installation-of-software}

Install the software:

1. Xcode tools  `xcode-select --install`  
2. Iterm2  
3. Ohmyzsh  Use:   
   `(sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)")`  
4. Zoom  
5. Rectangle.app 

Optional:

1. Chrome

# Setting up keys {#setting-up-keys}

As a software developer, you should have your private key that you keep to yourself, even if you change machines. It is personal to you ? as much as your gmail password is. If you need to use a different machine, copy your private key.

Tasks:

1. Generate / Copy private key to keep in id\_rsa \[You do not need to set a passphrase, which interferes with  automated activity\]  
2. Keep the public key in github.com. That is going to be your public key that you can download from any place.   
    `curl https://github.com/<yourgithubid>.keys`  
3. Keep your private key carefully. This will unlock any account that you lock with your public key.

4. Learn how to configure ssh configuration in .ssh with file `config`. Here is a small note:  
   `Host *`  
     	`ServerAliveInterval 30`  
   `Host <simple>`  
   	`HostName <fullyqualifiedname>`  
   	`Port <portnumber>`  
   	`User <userid>`

Now you can login with the simple name `ssh simple` without giving the fullname. Notice that you can manage many private keys, if you have them using this ssh config file. 

You will use this technique when you provision your own Unix machine in the lab.

# Installing homebrew {#installing-homebrew}

Install homebrew in userspace, which lets a lot of Unix tools be installed in homebrew.

`cd ~`  
`mkdir homebrew`  
`curl -L https://github.com/Homebrew/brew/tarball/master | tar xz --strip 1 -C homebrew`  
`export PATH=$HOME/homebrew/bin:$PATH # Add this to ~/.zshrc or ~/.bashrc`  
`brew update`

After installing homebrew, try the following:

1. pandoc  
2. bat (a drop in replacement for cat. You can do the alias `cat=bat` to see it in action.   
3. wget   
4. python (since system python is lame :-)). We will install python later.  
5. envsubst

# Python and Python Version Manager Installation {#python-and-python-version-manager-installation}

This section walks you through the installation of pyenv and friends. 

Prereq: Having python3 on your system (No, I am not talking about miniconda, I mean your system python). If you do, you are done.

However, I hear that python may not be installed by default in the future. So, do the following:

`brew install python`  
`cd $HOME/homebrew/bin`  
`ln python3 python`

If you test your system with the command type python it should show that it is using $HOME/brew/bin/python. If you test with python \-V it should show version 3.x.y

Install pyenv with brew install pyenv && brew install pyenv-virtualenv Then, you will have to setup pyenv with the following commands (and add to the .zshrc):

`export PYENV_ROOT="$HOME/.pyenv"`  
`export PYENV_HOME=/Users/$USER`  
`export PYENV_ROOT="$PYENV_HOME/.pyenv"`  
`export PATH="$PYENV_ROOT/bin:$PATH"`  
`if command -v pyenv 1>/dev/null 2>&1; then`  
    `eval "$(pyenv init -)"`  
    `eval "$(pyenv init --path)"`  
    `eval "$(pyenv virtualenv-init -)"`  
`fi`  
`export PYENV_LOCAL_SHIM="$HOME/.pyenv_local_shim"`  
`export PATH="$PYENV_LOCAL_SHIM:$PYENV_ROOT/bin:$PATH"`  
At this point, pyenv is all set up. Now, you can install any version of python.  Test Pyenv with the following commands:

* Install a new python interpreter with the command pyenv install 3.11.0  
* Make it as default by pyenv global 3.11.0   
* Now, test it by running python \-V ; you should see 3.9.9 as the python you are running.  
* Install pyenv install 3.10.3 as well.   
* Type pyenv versions to see what all versions are there.

## Managing virtual environments using pyenv {#managing-virtual-environments-using-pyenv}

In reality, we will be using poetry which makes virtualenv obsolete. However, for legacy reasons, we can make them work too. Here is the simple way. We already setup virtual env. All you need to do is the following:

pyenv virtualenv \<python-version\> \<name\>

For instance, you can do:

`mkdir myprj1`  
`pyenv virtualenv myproj1python 3.9.11 # Assuming you installed 3.9.11 using pyenv`  
`cd myprj1`  
`pyenv local myproj1python`  
`# Now whenever you cd to this directory, you use that python.`

Let us pause and see what all we can do:

1. You can install any version of Python you want.  
2. You can create a virtualenv on any version of python that you want.  
3. You can explicitly use that version of python or implicitly set using pyenv local. 

Finally, you can use poetry too ? which you will learn in pycharm.

# UV Python Version Manager Installation(new) {#uv-python-version-manager-installation(new)}

 

`curl -LsSf https://astral.sh/uv/install.sh | sh`

`For more information: https://docs.astral.sh/uv/getting-started/installation/`

To install python using uv 

[`https://docs.astral.sh/uv/guides/install-python/`](https://docs.astral.sh/uv/guides/install-python/)

## Managing projects using uv {#managing-projects-using-uv}

[`https://docs.astral.sh/uv/guides/projects/`](https://docs.astral.sh/uv/guides/projects/)

# Installing node {#installing-node}

We are going to install nvm for nodejs:

Install nvm and friends.  
`curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.3/install.sh | bash`

`exec $SHELL or close and reopen the terminal session`

`nvm install ?-lts`

`nvm use node`

Verification:

1. node \-p process.arch   
   You should see arm64  
2. node \-v && npm \-v  
   You should see the latest versions.

# Setting up vscode {#setting-up-vscode}

VScode is the general purpose editor of our choice. Here is what to install. 

1. Install VSCode  
2. Install VSCode shell command `code`. You can open a project folder or a file with the command code folder or code filename.   
3. Install the following extensions  
   1. remote ssh ( https://code.visualstudio.com/docs/remote/ssh )  
   2. Prettier (formats code, and can do well)  
   3. Liveshare (Collaborative editing and debugging)  
   4. Intellicode (for autosuggestions)  
   5. Bracket Pair Colorizer 2 (better bracket management)  
   6. Better Comments (comments for code management)  
   7. Markdown All-in-one (for writing documentation)  
   8. Python

Check if you can do the following:

1. Using the ssh config file that you set up earlier, you can open files and folders on the remote machine.  
2. Using the remote ssh feature, open a terminal window in vscode on the remote machine.  
3. Using remote ssh, access a port on the remote machine, without installing any additional software or setup.  
4. From iterm open a file in vscode  
5. Use git from VS Code: add files, edit files, and add commit messages and push the files to github.

For JavaScript editing, there are other extensions. For python, we prefer to use pycharm.

# Setting up Pycharm {#setting-up-pycharm}

Install the latest version pycharm community edition. After that, install the following plugins:

1. GitToolBox  
2. Makefile language  
3. GitLens  
4. GitLink  
5. DBNavigator  
6. Codetogether (even with the limitation of 1hr at a time, it is useful to code together).

These are found to be most useful for our work. 

You should be able to do the following:

1. Setup a python project  
2. Interpreter in pycharm ? you can choose Poetry  
3. Install packages automatically  
4. Browse through and eliminate all the errors and warnings.  
5. Run/Debug python projects  
6. Code together with another person from pycharm  
7. Invoke pycharm from the command line, in a specific folder.

# Introduction to our machines {#introduction-to-our-machines}

We have the following servers in our system. All have the FQDN with .aganitha.ai prefixed

| Name | Details | Purpose | Access |
| :---- | :---- | :---- | :---- |
| Own1 | RTX 4090 GPU/64GB/32threads | GPU, general purpose | LDAP. Direct on the internet. Port 2222, 80, 443 |
| Own2 | 1080 TI GPU/64gb/22 TB disk/ 8 threads | NFS, GPU computing, general purpose | LDAP. Direct on the internet. Port 2222, 80, 443\.  |
| Own3 | In Hyderabad, 2 GPUs and 12 threads | GPU, general purpose, NFS | LDAP. Direct on the internet. Port 2322, 80,443 |
| Own4 | One gpu, 32 threads, 128GB memory  machine | GPU, general purpose, NFS | LDAP. Direct on the internet. Port 2222, 8000, 8443 |
| own5 | 4090\. 32 threads. 128gb memory | GPU, general purpose, NFS | LDAP. Direct on the internet. Port 2522, 8500,8543 |
| own6 | 4090\. 32 threads. 128gb | GPU, general purpose | LDAP. Direct on the internet. Port 2622, 8600,8643 |
| hpc2 | No GPU. 64 threads. 512 GB | CPU | ssh 2222 and https |
| hpc3 | No GPU, 80 threads, 512 GB | CPU | ssh 2222 and https |
| hpc4 | No GPU, 56 threads, 128GB | CPU | ssh 2222 and https |

 **Own1, Own5** run **Ubuntu 24.10**. **Own2** runs **Ubuntu 22.04.5 LTS**.  **Own3, Own4, Own6, hpc3, hpc4** run **Ubuntu 24.04.2 LTS**.


# Git: basics you need to know {#git:-basics-you-need-to-know}

We are using github for our corporate use. Here is the way you work:

* Ask for your project to be created with the right permissions. Since this is a flat hierarchy, do  not break it down to too small a project.   
* Make sure you start with the right template. Especially with the right .gitignore. If your template does not provide one, look for it here: [https://github.com/github/gitignore](https://github.com/github/gitignore). We do not want to check generated, transient files to git. 

On branching:

1. Do not create branches unnecessarily. You can work on your code and make it work and update the main branch.  
2. Usually, it is best to have just one branch ? main. If you are making a release, tag the release so that you can get that release whenever you want.   
3. The only justification for multiple parallel branches is when the same code is being used by different customers. Even then, try to manage with configuration even with different code bases.  
4. Sometimes we have main and dev branches. Do this only if your main branch is in production. That way, you can make quick fixes to the main branch while we continue to develop the dev branch.

Here is the usual workflow:

`git clone <url> # ssh based. Works because you have your ssh key setup`  
`? edit your code ?`  
`? test your code ?`  
`Git add .`  
`Git commit -m ?add a commit statement. Make it around 2 to 3 lines.?`  
`Git push` 

There is a lot more to learn. Start with [GIT CHEAT SHEET](https://education.github.com/git-cheat-sheet-education.pdf) ; if you are using it from the command line, you can use it with zsh. If you are using `ohmyzsh`, you can check out plugins for git and git extras etc.

We already have seen how to use it from vscode and pycharm. For advanced material, you can turn to google.

Note to remember: The default branch is main, not master, in github. When you are reading and practising a tutorial, watch out for it. 

# Installing Aganitha Tool Kit (atk) {#installing-aganitha-tool-kit-(atk)}

We have not updated the aganitha tool kit for a long time. The new version, which is continually updated can be installed as follows:

`(cd /tmp;rm -rf atk-setup; git clone git@github.com:aganitha/atk-setup.git; bash atk-setup/bin/atk-install.sh; rm -rf atk-setup)`

# Standards {#standards}

Here are the standards for different names:

Assume that the project name is ?prj?. If there no project, then internal

1. Docker containers will be called atk-prj-\*  
2. Github Projects called prj-\* ? the github projects are named like that.

Each github project should have a README.md.x

The heading of the README.md should be as follows:

`?-`  
`project_name: ATK System tools`  
`short_description: System tools used for maintaining the servers`  
`tags: [scripts, internal]`  
`category: tech`  
`status: active`  
`long_description: >`  
  `These tools are used to set up the machines, add users, and automate maintenance.`  
  `There may be hardcoded details in here.`  
`?-`

# Appendix: {#appendix:}

## Setup your personal virtual machine (only if needed) {#setup-your-personal-virtual-machine-(only-if-needed)}

For most developers, having a VM locally helps in testing the applications. We can do a lot of root level activities, including running our own traefik, containers and such. 

For m1, we recommend the following:

1. Get the app from [https://mac.getutm.app/](https://mac.getutm.app/) ? not from the mac store. That costs money. You can get the same from the link provided.  
2. Download the VM you want: [https://medium.com/@lizrice/linux-vms-on-an-m1-based-mac-with-vscode-and-utm-d73e7cb06133](https://medium.com/@lizrice/linux-vms-on-an-m1-based-mac-with-vscode-and-utm-d73e7cb06133) ? we suggest to run Ubuntu 22.04 server, without GUI.

While this is optional, if you are going to deploy on Linux, we urge you to set up your own VM.

For Intel with MacOS, do the following:

1. Download and install virtualbox.  
2. Install Ubuntu 22.04 in that ? again, run it without GUI. 

