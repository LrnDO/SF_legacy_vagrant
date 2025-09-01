Vagrant.configure("2") do |config|
  config.vm.box = "generic/ubuntu1004"
  
  config.vm.provider "virtualbox" do |vb|
    vb.memory = "2048"
    vb.cpus = 1
    vb.name = "postgre8.4-srv"
  end

  config.vm.network "private_network", ip: "192.168.0.10"
  config.vm.hostname = "postgre8.4"

  config.vm.provision "shell", inline: <<-SHELL
    apt-get update
    apt-get install -y postgresql-8.4 
    apt-get install -y postgresql-client-8.4
    sed -i "s/#listen_addresses = 'localhost'/listen_addresses = '*'/" /etc/postgresql/8.4/main/postgresql.conf
    echo "host all all 0.0.0.0/0 md5" >> /etc/postgresql/8.4/main/pg_hba.conf
    service postgresql-8.4 restart
    sudo -u postgres psql -c "CREATE USER vagrant WITH PASSWORD 'vagrant';"
    sudo -u postgres psql -c "CREATE DATABASE vagrant OWNER vagrant;"
  SHELL
end
