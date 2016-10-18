# Vagrant

### mount host folder
    
    config.vm.synced_folder "vagrant_data" ,"/srv/website"

### save change
    you don't want to exec every command after reboot, so you can save change to a box file

    vagrant package
    
or

    vagrant package existing_instance_name --output new_instance_name.box

tips for minimize the box file
    
    sudo apt-get clean # useful
    sudo dd if=/dev/zero of=/EMPTY bs=1M #useful, but take time
    sudo rm -f /EMPTY # useful

    cat /dev/null > ~/.bash_history && history -c # for share ,necessary 
    
    [ref](https://scotch.io/tutorials/how-to-create-a-vagrant-base-box-from-an-existing-one)

### basic command

    vagrant init os_name 
    vagrant ssh your_vm_name
    vagrant status
    vagrant up 
    vagrant reload
   
### problem solving

#### getst doesn't support configure_netowrks
    
    Vagrant attempted to execute the capability 'configure_networks' on the detect guest OS 'linux', but the guest doesn't support that capability.  

solved:

    `config.vm.network "private_network", ip: "192.168.33.10"`

 - [ref](https://github.com/mitchellh/vagrant/issues/6426)
 - [ref](https://github.com/mitchellh/vagrant/issues/6829)

####  Vagrant was unable to mount VirtualBox shared folders 
   
    Vagrant was unable to mount VirtualBox shared folders. This is usually
    because the filesystem "vboxsf" is not available. This filesystem is
    made available via the VirtualBox Guest Additions and kernel module.
    Please verify that these guest additions are properly installed in the
    guest. This is not a bug in Vagrant and is usually caused by a faulty
    Vagrant box. For context, the command attemped was:

    set -e
    mount -t vboxsf -o uid=`id -u vagrant`,gid=`getent group vagrant | cut -d: -f3` vagrant /vagrant
    mount -t vboxsf -o uid=`id -u vagrant`,gid=`id -g vagrant` vagrant /vagrant

    The error output from the command was:

    /sbin/mount.vboxsf: mounting failed with the error: No such device
    
solved:

    `sudo ln -s /opt/VBoxGuestAdditions-4.3.10/lib/VBoxGuestAdditions /usr/lib/VBoxGuestAdditions
    vagrant plugin install vagrant-vbguest`

 - [ref](http://stackoverflow.com/questions/27992354/vagrant-error-failed-to-mount-folders-in-linux-guest-after-halt-or-reload/27992355#27992355)
 - [ref](http://stackoverflow.com/questions/22717428/vagrant-error-failed-to-mount-folders-in-linux-guest)
 - [ref](https://www.virtualbox.org/ticket/12879)

