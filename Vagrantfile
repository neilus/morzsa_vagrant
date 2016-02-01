## set ft=ruby :
#
Vagrant.configure(2) do |config|
  # enable this, if you install landrush vagrant plugin via
  # vagrant plugin install vagrant-landrush
  # for dnsmasq-ing your vagrant VMs
  #config.landrush.enabled = true

  # sample ubuntu environment. runs nothing, only for testing different
  # providers
  config.vm.define 'pogacsa', primary: true do |pogacsa|
    pogacsa.vm.hostname = 'pogacsa'

    # default virtualbox provider
    pogacsa.vm.provider :virtualbox do |vb, override|
        virtualbox.vm.box = "ubuntu/trusty64"
    end

    # local libvirt provider
    pogacsa.vm.provider :libvirt do |libvirt|
        # you probably have no libvirt box installed, convert the default
        # ubuntu into a libvirt as follows
        # $ sudo apt-get install qemu-utils libvirt-dev
        # $ vagrant plugin install vagrant-mutate
        # $ vagrant box add ubuntu/trusty64
        # $ vagrant mutate ubuntu/trusty64 libvirt
        libvirt.vm.box = "ubuntu/trusty64"
        #libvirt.driver = 'kvm' # or 'qemu' this is optional
    end

    # sampla docker provider entry
    # in this case the container will be started and stops right away, because
    # it has nothing to run
    pogacsa.vm.provider :docker do |docker, override|
      # no box needed, specify an image which will be downloaded from dockerhub
      docker.image = "ubuntu:trusty"
      docker.remains_running = true
    end

    pogacsa.vm.provider :openstack do |os, override|
      os.server_name = 'pogacsa'
      os.openstack_auth_url = 'http://128.136.179.2:5000/v2.0' # trystack.org
      os.username =
      os.password =
      os.tenant_name =
      os.flavor = 'm1.small'
      # no box needed, instead specify an image name or id from your openstack
      os.image = 'ubuntu14.04-LTS'
      #os.networks = ['private'] # [ 'vm_net', 'priv_net' ]
      #os.floating_ip =
      #os.floating_ip_pool = 'public'
      #os.floating_ip_pool_always_allocate = true
      #os.keypair_name =
      override.ssh.username = 'ubuntu' # in the openstack ubuntu image, the
                                       # default username is not vagrant
      override.ssh.private_key_path = '~/.ssh/id_rsa'
    end


    # sample openvz container on a proxmox host
    pogacsa.vm.provider :proxmox do |proxmox, override|
      #proxmox.endpoint = 'https://proxmox.host:8006/api2/json'
      #proxmox.user_name =
      #proxmox.password =

      proxmox.vm_name_prefix = 'vagrant_'
      # box is optional, since vagrant 1.6...
      #override.vm.box = 'dummy'
      # spcify a vagrant-compatible openvz template on the proxmox host
      # which needs to have a vagrant user...
      proxmox.openvz_os_template = 'local:vztmpl/vagrant-proxmox-ubuntu-12.tar.gz'
      proxmox.vm_type = :openvz
      proxmox.vm_memory = 256
      #override.ssh.username = 'ubuntu'
      #override.ssh.private_key_path = '~/.ssh/id_rsa'
    end
  end


  # sample remote libvirt VM
  config.vm.define 'cookie', autostart: false  do |cookie|
    cookie.vm.hostname = 'cookie'
    cookie.vm.provider :libvirt do |libvirt|
      libvirt.driver = 'kvm'
      libvirt.host = # this hostname is the same as in ~/.ssh/config
      libvirt.username = # have to specify username explicitly
      libvirt.connect_via_ssh = true # this is mandatory in case of a remote
                                     # host, in order to automatically make
                                     # vagrant redirect necessary ports like
                                     # ssh
      libvirt.id_ssh_key_file = "$HOME/.ssh/id_rsa" # explicitly specify ssh
                                                    # private key to use
    end
  end

  # this is a sample docker env running a webserver
  # run vagrant up nginx and you can reach it on
  # http://localhost
  config.vm.define 'nginx', autostart: false do |nginx|
  # Configure the Docker provider for Vagrant
    nginx.vm.provider "docker" do |docker|

      # Specify the Docker image to use
      docker.image = "nginx"

      # Specify port mappings
      # If omitted, no ports are mapped!
      docker.ports = ['80:80', '443:443']

      # Specify a friendly name for the Docker container
      docker.name = 'nginx-container'
    end
  end
end
