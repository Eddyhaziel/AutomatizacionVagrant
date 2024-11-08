Vagrant.configure("2") do |config|

    # Servidor Apache 1
    config.vm.define "web1" do |web1|
      web1.vm.box = "ubuntu/bionic64"
      web1.vm.hostname = "web1"
      web1.vm.network "private_network", ip: "192.168.33.10"
      web1.vm.provider "virtualbox" do |vb|
        vb.memory = "512"
        vb.cpus = 1
      end
      web1.vm.synced_folder "./web1", "/var/www/html"
      web1.vm.provision "shell", inline: <<-SHELL
        sudo apt-get update
        sudo apt-get install -y apache2
        sudo systemctl enable apache2
        echo '<h1>Servidor Apache 1</h1>' | sudo tee /var/www/html/index.html
      SHELL
    end
  
    # Servidor Apache 2
    config.vm.define "web2" do |web2|
      web2.vm.box = "ubuntu/bionic64"
      web2.vm.hostname = "web2"
      web2.vm.network "private_network", ip: "192.168.33.11"
      web2.vm.provider "virtualbox" do |vb|
        vb.memory = "512"
        vb.cpus = 1
      end
      web2.vm.synced_folder "./web2", "/var/www/html"
      web2.vm.provision "shell", inline: <<-SHELL
        sudo apt-get update
        sudo apt-get install -y apache2
        sudo systemctl enable apache2
        echo '<h1>Servidor Apache 2</h1>' | sudo tee /var/www/html/index.html
      SHELL
    end
  
    # Servidor Nginx (Load Balancer)
    config.vm.define "nginx" do |nginx|
      nginx.vm.box = "ubuntu/bionic64"
      nginx.vm.hostname = "nginx"
      nginx.vm.network "private_network", ip: "192.168.33.12"
      nginx.vm.provider "virtualbox" do |vb|
        vb.memory = "512"
        vb.cpus = 1
      end
      nginx.vm.provision "shell", inline: <<-SHELL
        sudo apt-get update
        sudo apt-get install -y nginx
        sudo systemctl enable nginx
        # Configurar Nginx como Load Balancer
        sudo bash -c 'cat > /etc/nginx/sites-available/default' <<EOF
  upstream backend {
      server 192.168.33.10;
      server 192.168.33.11;
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
  