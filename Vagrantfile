Vagrant.configure("2") do |config|
    # Configuration globale pour toutes les VMs
    config.vm.box = "debian/bullseye64"

    # VM3 : Routeur
    config.vm.define "routeur" do |routeur|
        routeur.vm.network "private_network", ip: "192.168.255.100" # Interface LAN
        routeur.vm.network "public_network"
        routeur.vm.hostname = "routeur"
    
        # Définir la taille de la mémoire (en Mo)
        routeur.vm.provider "virtualbox" do |vb|
            vb.memory = 512  # 512 Mo de RAM
        end
    
        routeur.vm.provision "shell", inline: <<-SHELL
                # Définir la gateway
                ip route delete default || true
                ip route add default via 172.16.117.254
                echo "Default route modified..."
    
                # Activer le routage IP
                echo "net.ipv4.ip_forward=1" >> /etc/sysctl.conf
                sysctl -p
                echo "Routing enabled..."
    
                # Configurer le NAT avec iptables
                iptables -t nat -A POSTROUTING -o eth2 -j MASQUERADE
                echo "Nat Dynamic configured on eth2 for out interface..."
        SHELL
    end
  
    # VM1 : Client
    config.vm.define "client" do |client|
        client.vm.network "private_network", ip: "192.168.255.10"
        client.vm.hostname = "client"

        client.vm.synced_folder "./capture", "/home/vagrant/capture"
        # Définir la taille de la mémoire (en Mo)
        client.vm.provider "virtualbox" do |vb|
            vb.memory = 512  # 512 Mo de RAM
        end

      # Provisionnement par shell
        client.vm.provision "shell", inline: <<-SHELL
            # Définir la gateway
            ip route delete default || true
            ip route add default via 192.168.255.254
            echo "Default route modified..."
            # Install TCPDump
            sudo apt update
            sudo apt install -y tcpdump
            echo "tcpdump installed..."
        SHELL
    end
  
    # VM2 : Serveur
    config.vm.define "fakerouter" do |fakerouter|
        fakerouter.vm.network "private_network", ip: "192.168.255.254"
        fakerouter.vm.hostname = "fakerouter"

        # Définir la taille de la mémoire (en Mo)
        fakerouter.vm.provider "virtualbox" do |vb|
            vb.memory = 512  # 512 Mo de RAM
        end

        # Définir la gateway
        fakerouter.vm.provision "shell", inline: <<-SHELL
            ip route delete default || true
            ip route add default via 192.168.255.100

            # Activer le routage IP
            echo "net.ipv4.ip_forward=1" >> /etc/sysctl.conf
            sysctl -p
            echo "Routing enabled..."
        SHELL
    end

end
  