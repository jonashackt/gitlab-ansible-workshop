# Prerequisites

## VirtualBox, Vagrant, git, Ansible

First we need some tools!

### Mac

We´re using [homebrew](https://brew.sh/index_de) package manager here:

```
$ brew cask install virtualbox
$ brew cask install vagrant
$ brew install git
$ brew install ansible
```

### Windows

#### chocolatey & WSL

Bit more complicated, since Ansible doesn´t support Windows as the control machine. But there´s the [Windows Linux Subsystem (WLS)](https://docs.microsoft.com/en-us/windows/wsl/install-win10) ftw! 

First we install VirtualBox, Vagrant & git with the package manager [chocolatey](https://chocolatey.org/):

```
$ Set-ExecutionPolicy Bypass -Scope Process -Force; iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))
```

Now chocolatey is present, install

```
$ choco install virtualbox
$ choco install vagrant
$ choco install git
```

Now we need to configure WLS:

```
Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Windows-Subsystem-Linux
```

Now search the Windows Store for Ubuntu. Install it with __GET__. Press __LAUNCH__ then:

![Installing_Ubuntu_on_Control_Machine.png](https://github.com/jonashackt/ansible-linux-windows-workshop/blob/master/Installing_Ubuntu_on_Control_Machine.png)

Now choose username and password.

Then update packages via

```
sudo apt-get update
```

#### Configure Ansible Control Machine on WSL

Siehe http://docs.ansible.com/ansible/latest/intro_installation.html#latest-releases-via-apt-ubuntu

```
sudo apt-add-repository ppa:ansible/ansible
sudo apt-get update
sudo apt-get install ansible
```

![Installing_Ansible_on_Control_Machine.png](https://github.com/jonashackt/ansible-linux-windows-workshop/blob/master/Installing_Ansible_on_Control_Machine.png)









#### All

```
$ vagrant plugin install vagrant-dns
```



