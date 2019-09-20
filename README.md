## APM / Elastic Search / Kibana STACK:

### GET THE SYSTEM RUNING LOCALLY:
#### Requirements:
Before we get started with the cool stuff => getting all the systems running on your own machine locally, we have to make sure that
the following requirements are properly installed:
* [VirtualBox](https://www.virtualbox.org/wiki/Linux_Downloads)
* [Vagrant](https://www.vagrantup.com/)

##### Cloning the Project:
Once all the requirements installed, we have to clone the project:

``` sh
$ git https://github.com/moda20/VagrantDockerElasticStack.git
```

*NOTE*: I generally create all my vagrant projects under __/$HOME/VagrantVms/__.

#### Running Vagrant:
##### Vagrantfile:
The primary function of the Vagrantfile is to describe the type of machine required for a project,
and how to configure and provision these machines.

###### Defining VMs:

All the vagrant machines are based on *bento/debian-10* image:
``` ruby
# Vagrantfile

...

BOX_IMAGE = "bento/debian-10"

...
```
Here we declared a variable __BOX_IMAGE__ which we are going to use in each machine.

*NOTE*: The *Vagrantfile* is a Ruby file.

In this project we have a since VM with Three Docker containers each Container is defined
like the following:
``` ruby
# Vagrantfile

...

  config.vm.synced_folder "apm_apm", "/home/vagrant/apm_apm"
  
  config.vm.provision "docker" do |docker|
  
    ...
        docker.build_image "/home/vagrant/apm_apm/",
          args: "-t apm_apm"
        docker.run "apm_apm",
          args: "-p 8200:8200"   
    ...
    
  end
  
...
```

The definition is not exactly the same for all docker containers, but it follows the same concept of 
synchronizing folder and then creating then building then running the container.


Here we are assigning *"apmapm"* as hostname and finally created a *private_network* to allow host-only access using
the specific IP *"192.168.30.10"*.

``` ruby
# Vagrantfile

...

  config.vm.hostname = "apmapm"
  config.vm.network "private_network", ip: "192.168.30.10"
  
...
```
Synced folders enable Vagrant to sync a folder on the
host machine to the guest machine which means that all the files in __apm_apm(local)__ folder will be copied
and synchronized with __/home/vagrant/apm_apm(VM)__.

*NOTE*: Any changes will be reflected in both sides instantly if you are using *VirtualBox* as hypervisor,
otherwise you need to reload the VM.

###### Provisioning:

TO BE COMPLETED SOON


###### Running the VMs:

Now that we have everything locally, we can run vagrant to create all the different VMs:

```
$ vagrant up
```

To bring up a single VM:
```
$ vagrant up $machine_name
```

If you've changed the provisionning of any machine you need to reload(restart) it:
```
$ vagrant reload $machine_name --provision
```
*NOTE*: $machine_name is optional, if it's not specified all VMs in Vagrantfile will be reloaded.

Other useful vagrant commands:
```
$ vagrant halt           # Brings down all the VMs defined in the Vagrantfile
$ vagrant destroy        # Halts and remove the VMs
$ vagrant global-status  # Shows the state of all active Vagrant environment
$ vagrant --help         # Prints help for all vagrant commands
```
