# Vagrant

### mount host folder
    
    config.vm.synced_folder "vagrant_data" ,"/srv/website"

### save change
    you don't want to exec every command after reboot, so you can save change to a box file

    vagrant package
    
    or

    vagrant package existing_instance_name --output new_instance_name.box


