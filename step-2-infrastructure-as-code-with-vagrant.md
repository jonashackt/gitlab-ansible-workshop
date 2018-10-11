# Step 2: Infrastructure-as-Code with Vagrant

## Our first Vagrant Box

First things first: we need a server for GitLab! That´s not a real problem since we´re using Vagrant here.

Let´s create a simple `Vagrantfile`:

```text
Vagrant.configure("2") do |config|

    config.vm.box = "ubuntu/bionic64"

    config.vm.define "first-vagrant-box"

    config.vm.provider :virtualbox do |virtualbox|
        virtualbox.gui = true
        virtualbox.memory = 512
        virtualbox.cpus = 1
    end
end
```

Save the file and execute the following command in the same directory:

`vagrant up`

What´s happening? Why is that cool?

## A Vagrant Box for GitLab

Now let´s edit and enhance our Vagrantfile, so that the specific needs of a GitLab instance are met.

Therefore we should purge the initial VagrantBox first with `vagrant destroy -f`

Then we should enhance our Vagrantfile: We want to get rid of the generated VirtualBox-VM name like `gitlab-ansible-workshop_first-vagrant-box_1539178779571_72687`

### VirtualBox and VagrantBox name

Therefore use the [Vagrant docs](https://www.vagrantup.com/docs/vagrantfile/) to configure the VirtualBox´ name to `gitlab-ci-stack`

As this only changes the VirtualBox VM name, we also want to change the VagrantBox name. This could be done with the following code:

`config.vm.define "gitlab-ci-stack"` \(don´t use a "=" here!\)

### Configure IP

As we aim to create code in an Infrastructure-as-Code manner, we not only want to be able to use the resulting code in a localhost environment. But also with "real" servers. Therefore we should configure a "real" network including an IP!

In Vagrant there´s a `config.vm.network` keyword to configure networking. Have a look at [https://www.vagrantup.com/docs/networking/](https://www.vagrantup.com/docs/networking/) and configure a static IP `172.16.2.15` for our VagrantBox.

### More Power!

Our VagrantBox isn´t really capable of running a full blown GitLab CI setup right now. So let´s give it more power! Configure our Box to have `4096` MB of RAM & `2` CPUs.

### Domain & DNS preparations

There´s one central thing a GitLab installation is centered: a domain name! To configure a domain and a top level domain \(tld\) name in Vagrant together with the [vagrant-dns plugin](https://github.com/BerlinVagrant/vagrant-dns), we need to use the following code:

```text
# Register domain and tld for later access prettiness (working with vagrant-dns Plugin https://github.com/BerlinVagrant/vagrant-dns)
config.vm.hostname = "jonashackt"
config.dns.tld = "io"
```

We choose the domain name [jonashackt.io](https://jonashackt.io) here, since throughout the workshop we´ll need administrative + API access to a domain registrar to obtain valid [Let´s Encrypt](https://letsencrypt.org/) certificates. If you have both admin and API access to a domain of your choice and your DNS provider [is present on this list](https://github.com/AnalogJ/lexicon#providers), feel free to use your own domain name here.

As we also want to have our domain name accessible inside our Vagrant Box, we need to forward our DNS resolver from our host right into the Box. This is [currently not possible for all Vagrant providers](https://www.vagrantup.com/docs/virtualbox/common-issues.html#dns-not-working), but luckily [with VirtualBox it is](https://serverfault.com/questions/453185/vagrant-virtualbox-dns-10-0-2-3-not-working/506206#506206). Therefore add the following line to the Vagrantfile:

```text
# Forward DNS resolver from host (vagrant dns) to box
virtualbox.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
```

### Configure DNS & fire up the Box

Now our `Vagrantfile` should be ready for our fully features GitLab. We only need to use the vagrand-dns plugin to do it´s DNS magic:

```
vagrant dns --install
```

Let´s check with `scutil --dns` (on a Mac), if the resolver is part of your DNS configuration:

```
...

resolver #10
  domain   : io
  nameserver[0] : 127.0.0.1
  port     : 5300
  flags    : Request A records, Request AAAA records
  reach    : 0x00030002 (Reachable,Local Address,Directly Reachable Address)

...
```

If that looks good, we´re ready to fire up the Vagrant Box with `vagrant up`.

After the successful startup, we should try to reach our Vagrant Box using our defined domain: `dscacheutil -q host -a name gitlab.jonashackt.io` This should look like the following:

```
$ dscacheutil -q host -a name gitlab.jonashackt.io
  name: gitlab.jonashackt.io
  ip_address: 172.16.2.15
```
