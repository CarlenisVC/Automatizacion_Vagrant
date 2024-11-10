Vagrant.configure("2") do |config|
    # Configuración de la primera máquina con Apache
    config.vm.define "web1" do |web1|
      web1.vm.box = "ubuntu/bionic64"
      web1.vm.network "private_network", ip: "192.168.56.101"
      web1.vm.provider "virtualbox" do |vb|
        vb.memory = "512"
        vb.cpus = 1
      end
      web1.vm.provision "shell", inline: <<-SHELL
        sudo apt-get update
        sudo apt-get install -y apache2
        echo "<h1>Servidor Web 1</h1>" | sudo tee /var/www/html/index.html
      SHELL
      web1.vm.synced_folder ".", "/vagrant", type: "virtualbox"
    end
  
    # Configuración de la segunda máquina con Apache
    config.vm.define "web2" do |web2|
      web2.vm.box = "ubuntu/bionic64"
      web2.vm.network "private_network", ip: "192.168.56.102"
      web2.vm.provider "virtualbox" do |vb|
        vb.memory = "512"
        vb.cpus = 1
      end
      web2.vm.provision "shell", inline: <<-SHELL
        sudo apt-get update
        sudo apt-get install -y apache2
        echo "<h1>Servidor Web 2</h1>" | sudo tee /var/www/html/index.html
      SHELL
      web2.vm.synced_folder ".", "/vagrant", type: "virtualbox"
    end
  
    # Configuración de la máquina con NGINX para balanceo de carga
    config.vm.define "loadbalancer" do |loadbalancer|
      loadbalancer.vm.box = "ubuntu/bionic64"
      loadbalancer.vm.network "private_network", ip: "192.168.56.103"
      loadbalancer.vm.provider "virtualbox" do |vb|
        vb.memory = "512"
        vb.cpus = 1
      end
      loadbalancer.vm.provision "shell", inline: <<-SHELL
        sudo apt-get update
        sudo apt-get install -y nginx
        sudo tee /etc/nginx/conf.d/load_balancer.conf > /dev/null <<EOF
        upstream backend {
          server 192.168.56.101;
          server 192.168.56.102;
        }
        server {
          listen 80;
          location / {
            proxy_pass http://backend;
          }
        }
        EOF
        sudo systemctl restart nginx
      SHELL
    end
  end