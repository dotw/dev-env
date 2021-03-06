# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure(2) do |config|
  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://atlas.hashicorp.com/search.
  config.vm.box = "ubuntu/xenial64"
  config.vm.hostname = "devops"

  # Set up network port forwarding
  config.vm.network "forwarded_port", guest: 5000, host: 5000, host_ip: "127.0.0.1"
  config.vm.network "forwarded_port", guest: 8080, host: 8080, host_ip: "127.0.0.1"
  config.vm.network "forwarded_port", guest: 50000, host: 50000, host_ip: "127.0.0.1"
  config.vm.network "private_network", ip: "192.168.33.10"

  # Windows users need to change the permissions explicitly so that Windows doesn't
  # set the execute bit on all of your files which messes with GitHub users on Mac and Linux
  DIRNAME="DevOpsBootcamp"
  SOURCE_MOUNT="/#{DIRNAME}"
  config.vm.synced_folder "..", "#{SOURCE_MOUNT}", owner: "ubuntu", mount_options: ["dmode=755,fmode=644"]

  # Allocate machine
  config.vm.provider :virtualbox do |vb|
    vb.name = "devops"
    vb.memory = "1024"
    vb.cpus = 2
  end

  # Copy your .gitconfig file so that your git credentials are correct
  if File.exists?(File.expand_path("~/.gitconfig"))
    config.vm.provision "file", source: "~/.gitconfig", destination: "~/.gitconfig"
  end

  ######################################################################
  # Setup a Development environment for Python, Java, and Cloud Foundry
  ######################################################################
  
    # wget -q -O - https://packages.cloudfoundry.org/debian/cli.cloudfoundry.org.key | sudo apt-key add -
    # echo "deb http://packages.cloudfoundry.org/debian stable main" | sudo tee /etc/apt/sources.list.d/cloudfoundry-cli.list
    # apt-get update
    # # Install Python and Cloud Foundry API
    # echo "\n----- Installing Python / Cloud Foundry Development Environment ------\n"
    # apt-get install -y git zip tree python-pip python-dev build-essential cf-cli
    # pip install --upgrade pip
    # pip install virtualenv
    # sudo -H -u ubuntu bash -c "alias a=\'. venv/bin/activate\' >> ~/.bashrc"
    # # Install the IBM Container plugin as ubuntu
    # sudo -H -u ubuntu bash -c "echo Y | cf install-plugin https://static-ice.ng.bluemix.net/ibm-containers-linux_x64"
    # Add Java 8
    # # Install PhantomJS for Selenium browser support
    # echo "\n----- Installing PhantomJS Testing Environment ------\n"
    # apt-get install -y chrpath libssl-dev libxft-dev
    # apt-get -y autoremove
    # # from PhantomJS https://bitbucket.org/ariya/phantomjs/downloads/phantomjs-2.1.1-linux-x86_64.tar.bz2
    # cd ~
    # export PHANTOM_JS="phantomjs-2.1.1-linux-x86_64"
    # wget https://bitbucket.org/ariya/phantomjs/downloads/$PHANTOM_JS.tar.bz2
    # tar xvjf $PHANTOM_JS.tar.bz2
    # mv $PHANTOM_JS /usr/local/share
    # ln -sf /usr/local/share/$PHANTOM_JS/bin/phantomjs /usr/local/bin
    # rm -f $PHANTOM_JS.tar.bz2
    # Make vi look nice
  

  ######################################################################
  # Add Redis docker container
  ######################################################################
  # config.vm.provision "shell", inline: <<-SHELL
  #   # Prepare Redis data share
  #   sudo mkdir -p /var/lib/redis/data
  #   sudo chown ubuntu:ubuntu /var/lib/redis/data
  # SHELL

  # # Add Redis docker container
  # config.vm.provision "docker" do |d|
  #   d.pull_images "redis:alpine"
  #   d.run "redis:alpine",
  #     args: "--restart=always -d --name redis -h redis -p 6379:6379 -v /var/lib/redis/data:/data"
  # end

  # Add Jenkins docker container
  config.vm.provision "docker" do |d|
    d.pull_images "jenkins/jenkins:lts"
    d.run "jenkins/jenkins:lts",
      args: "--name jenkins -u root -p 8080:8080 -p 50000:50000 -v /vagrant/jenkins_home:/var/jenkins_home -v /var/run/docker.sock:/var/run/docker.sock -v $(which docker):/usr/bin/docker -v /usr/lib/x86_64-linux-gnu/libltdl.so.7:/usr/lib/libltdl.so.7"
  end  

  # # Add ZAP container
  # config.vm.provision "docker" do |d|
  #   d.pull_images "owasp/zap2docker-stable"
  #   d.run "owasp/zap2docker-stable zap.sh -daemon -host 0.0.0.0 -port 8080",
  #     args: "--name jenkins -u zap -p 8000:8080"
  # end  

  # # Add Tomcat docker container
  # config.vm.provision "docker" do |d|
  #   d.pull_images "tomcat:8.5-jre8-alpine"
  #   d.run "tomcat:8.5-jre8-alpine",
  #     args: "--restart=always -d --name tomcat -p 8080:8080 -v /vagrant/webapps:/usr/local/tomcat/webapps"
  # end

end
