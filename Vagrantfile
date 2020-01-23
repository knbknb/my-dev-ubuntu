Vagrant.configure("2") do |config|
  # https://docs.vagrantup.com

  # https://app.vagrantup.com/felipecassiors/boxes/ubuntu1804-4dev
  config.vm.box = "felipecassiors/ubuntu1804-4dev"

  # Expose VM port to host, enable public access  
  config.vm.network "forwarded_port", guest: 80,   host:  8888  # mdis Apache Webserver
  config.vm.network "forwarded_port", guest: 8080, host: 18080  # mdis Vue/Webpack devserver
  config.vm.network "forwarded_port", guest: 22,   host:  2222  # ssh
  config.vm.network "forwarded_port", guest: 3306, host: 33306  # mysql
 

  # Insert SSH key (but sadly this doesn't work on Windows Host with Git Bash)
  config.ssh.forward_agent = true

  # Workaround to forward ssh key from Windows Host with Git Bash
  if Vagrant::Util::Platform.windows?
    #if File.exists?("~\Documents\Upload")
      # config.vm.synced_folder "~\Documents\Upload", "/var/www/dis/backend/data/upload", automount: true
    #end
    if File.exists?(File.join(Dir.home, ".ssh", "id_rsa"))
        # Read local machine's SSH Key (~/.ssh/id_rsa)
        ssh_key = File.read(File.join(Dir.home, ".ssh", "id_rsa"))
        # Copy it to VM as the /vagrant/.ssh/id_rsa key
        config.vm.provision :shell, privileged: false, :inline => "echo 'Windows-specific: Copying local SSH Key to VM for provisioning...' && mkdir -p /home/vagrant/.ssh && echo '#{ssh_key}' > /home/vagrant/.ssh/id_rsa && chmod 600 /home/vagrant/.ssh/id_rsa", run: "always"
    else
        # Else, throw a Vagrant Error. Cannot successfully startup on Windows without a SSH Key!
        raise Vagrant::Errors::VagrantError, "\n\nERROR: SSH Key not found at ~/.ssh/id_rsa.\nYou can generate this key manually by running `ssh-keygen` in Git Bash.\n\n"
    end
  else
    #config.vm.synced_folder "Upload", "/var/www/dis/backend/data/upload", automount: true
    
  end

  # Share folder
  # config.vm.synced_folder "C:/", "/c/"
  config.vm.synced_folder ".", "/home/vagrant/"

  # Check if we run in a weird host
  require 'socket'
  gfz = Socket.gethostbyname(Socket.gethostname).last.unpack("C*").join(".")
  
  config.vm.provider "virtualbox" do |vb|
    # Display the VirtualBox GUI
    vb.gui = true

    # Set the display name of the VM in VirtualBox
    vb.name = "mdisdev"

    # Customize the amount of memory on the VM:
    vb.memory = "4096"

    # Customize the amount of CPU on the VM:
    if gfz
      vb.cpus = "1"
    else
      vb.cpus = "1"
    end

    # Make the DNS calls be resolved on host
    vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]

  end

  # Run a shell script in first run
  config.vm.provision "shell", privileged: false, inline: <<-SHELL
    set -euxo pipefail

    export DEBIAN_FRONTEND=noninteractive

    # Upgrade system
    sudo apt-get update
    sudo apt-get dist-upgrade -qq
    sudo apt-get autoremove -qq
    sudo snap refresh

    # Set keyboard layout to German 
    gsettings set org.gnome.desktop.input-sources sources "[('xkb', 'de')]"

    # Set timezone to Europe/Berlin
    sudo rm /etc/localtime && sudo ln -s /usr/share/zoneinfo/Europe/Berlin  /etc/localtime

    # Set default browser

    # Set git name
    if [ '#{gfz}' = true ]; then
      git config --global user.name "Knut Behrends"
      git config --global user.email "knb@gfz-potsdam.de"
    else
      git config --global user.name "Knut Behrends"
      git config --global user.email "knb@gfz-potsdam.de"
    fi

  SHELL

  # Check if SSH agent forward is working
  # config.vm.provision "shell", privileged: false, inline: "ssh -o StrictHostKeyChecking=no -T git@github.com", run: "always"

end
