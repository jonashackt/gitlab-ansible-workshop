# Prerequisites

## VirtualBox, Vagrant, git, Ansible

First we need some tools!

#### Mac

We´re using [homebrew](https://brew.sh/index_de) package manager here:

```
$ brew cask install virtualbox
$ brew cask install vagrant
$ brew install git
$ brew install ansible
```

#### Windows

Bit more complicated, since Ansible doesn´t support Windows as the control machine. But there´s the [Windows Linux Subsystem](https://docs.microsoft.com/en-us/windows/wsl/install-win10) ftw! 



Install the package manager [chocolatey](https://chocolatey.org/):

```
$ Set-ExecutionPolicy Bypass -Scope Process -Force; iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))
```





```
$ choco install virtualbox
$ choco install vagrant
$ brew install git
$ brew install ansible
```

#### All

```
$ vagrant plugin install vagrant-dns
```



