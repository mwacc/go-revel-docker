VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  # Always use Vagrant's default insecure key
  config.ssh.insert_key = false
  # ip address
  config.vm.network :private_network, ip: "192.168.56.100"
  # sync current folder
  config.vm.synced_folder ".", "/vagrant"
  # avoid getting stdin errors
  config.ssh.shell = "bash -c 'BASH_ENV=/etc/profile exec bash'"
  # Set RAM size
  config.vm.provider :virtualbox do |v|
    v.name = "go.revel.dev"
  	v.memory = 500
  	v.cpus = 2
  end
  
  config.vm.define "go-revel-img" do |z|
    z.vm.box = "ubuntu/trusty64"
    z.vm.hostname = "docker-host"
    z.vm.box_check_update = false
    z.vm.hostname = "go-revel"

    # Only run the provisioning on the first 'vagrant up'
    if Dir.glob("#{File.dirname(__FILE__)}/.vagrant/machines/default/*/id").empty?
      # Install Docker and some utilities
      pkg_cmd = "
             apt-get update;
             apt-get install -y apt-transport-https ca-certificates;
             apt-key adv --keyserver hkp://p80.pool.sks-keyservers.net:80 --recv-keys 58118E89F3A912897C070ADBF76221572C52609D;
             rm -rf /etc/apt/sources.list.d/docker.list;
             touch /etc/apt/sources.list.d/docker.list;
             echo deb https://apt.dockerproject.org/repo ubuntu-trusty main > /etc/apt/sources.list.d/docker.list;
             apt-get update -qq;
             apt-get purge lxc-docker;
             apt-cache policy docker-engine;
             apt-get install -y linux-image-extra-$(uname -r);
             apt-get update;
             apt-get install -y docker-engine;
             service docker start;
             apt-get install -y dos2unix;
             "

      # Add vagrant user to the docker group
      pkg_cmd << "usermod -a -G docker vagrant;"

      #Install docker-compose
      pkg_cmd << "apt-get -y install python-pip; pip install --upgrade pip; pip install docker-compose;"

      config.vm.provision :shell, :inline => pkg_cmd
    end
  end

  config.vm.post_up_message = "Congratulations! Now you can ssh into environment on host 192.168.56.100"

end
