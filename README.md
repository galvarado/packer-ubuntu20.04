# packer-ubuntu20.04
Packer template to build Ubuntu 20.04 image  for virtualbox (Vagrant). 

Packer template to build Ubuntu 20.04 image  for virtualbox (Vagrant). Ubuntu is discontinuing support for the Debian-installer based classic server installer from 20.04 LTS (Focal Fossa) making the way for subiquity server installer. This template use subiquity with cloud-init template to build the image.


## Requirements

- Install Packer to build the image:  https://www.packer.io/docs/install
- Install Vagrant to start a box with this  image: https://www.vagrantup.com/docs/installation

## How to build the image

1. Download the Ubuntu 20.04 iso image from https://releases.ubuntu.com/ and place it into the iso dir.

2. Copy the checksum form the image.

3. In the variables section, in the ubuntu2004.json template, modify the values of iso_checksum and iso_url to match the iso file name downloaded and the checksum:
```

  "variables": {
        "headless": "false",
        "iso_checksum": "",
        "iso_url": "",
        "version": "0",
        "ram": "1024",
        "cpus": "2",
        "virtualbox_disk_size": "12288"
    }
```

4. Buid the image:

```
$ packer build  ubuntu2004jupyter.json
```


5. The artifact will be placed at: 

```
output/ubuntu-20.04-virtualbox.box
```


## How to use it with Vagrant

1. After the build ends, add the box to vagrant:
```

$ vagrant box add --name ubuntu-20.04 output/ubuntu-20.04-virtualbox.box
```

2.  Create a Vagrantfile
```

$ cat > Vagrantfile << 'EOF'
# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|

  config.vm.box = "ubuntu-20.04"
  config.ssh.password = "vagrant"

  config.vm.provider "virtualbox" do |vb|
    # Display the VirtualBox GUI when booting the machine
    vb.gui = false
  end
end

EOF
```

3. Start the box and ssh into it
```

$ vagrant up && vagrant ssh
```

