Vagrant.configure("2") do |config|

    # Set up machine with OS ubuntu and static IP
    config.vm.define "machine" do |vb|
    vb.vm.box = "bento/ubuntu-18.04"
    vb.vm.network "private_network", ip: "192.168.56.50"

    # Set up public key and upload it to authorized_keys file for both vagrant and root user
    vb.vm.provision "shell" do |s|
        ssh_pub_key = File.readlines("#{Dir.home}/.ssh/id_rsa.pub").first.strip
        s.inline = <<-SHELL
            sudo mkdir /root/.ssh && sudo touch /root/.ssh/authorized_keys
    
            echo #{ssh_pub_key} >> /home/vagrant/.ssh/authorized_keys
            echo #{ssh_pub_key} >> /root/.ssh/authorized_keys

            # Disable ipv6
            sudo sysctl -w net.ipv6.conf.all.disable_ipv6=1 
            sudo sysctl -w net.ipv6.conf.default.disable_ipv6=1 
            sudo sysctl -w net.ipv6.conf.lo.disable_ipv6=1
            
            # Enable UFW firewall and allow port 22
            sudo ufw allow 22
            echo "y" | sudo ufw enable

            # Set up login with password and root
            sudo echo "PermitRootLogin yes" >> /etc/ssh/sshd_config
            sudo echo "PasswordAuthentication yes" >> /etc/ssh/sshd_config
    
            # Set up root password and restart ssh service
            sudo echo -e "parola\nparola" | passwd root
            sudo service ssh restart
    
            SHELL
        end
    end
  end