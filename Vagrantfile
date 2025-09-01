Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/focal64"

  config.vm.provider "virtualbox" do |vb|
    vb.memory = "1024"
    vb.cpus = 1
    vb.name = "postgresql-8.4-server"
  end

  config.vm.network "private_network", ip: "192.168.0.10"
  config.vm.hostname = "postgresql-8.4"

  config.vm.provision "shell", inline: <<-SHELL
    sudo apt-get update
    sudo apt-get upgrade -y
    sudo apt-get install -y build-essential libreadline-dev zlib1g-dev flex bison
    
    cd /tmp
    wget https://ftp.postgresql.org/pub/source/v8.4.22/postgresql-8.4.22.tar.gz
    tar -xzf postgresql-8.4.22.tar.gz
    cd postgresql-8.4.22
    
    ./configure --prefix=/usr/local/pgsql
    make
    make install
    
    sudo useradd postgres
    mkdir -p /usr/local/pgsql/data
    chown postgres:postgres /usr/local/pgsql/data
    sudo -u postgres /usr/local/pgsql/bin/initdb -D /usr/local/pgsql/data
    echo "listen_addresses = '*'" >> /usr/local/pgsql/data/postgresql.conf
    echo "host all all 0.0.0.0/0 md5" >> /usr/local/pgsql/data/pg_hba.conf
    sudo -u postgres /usr/local/pgsql/bin/pg_ctl -D /usr/local/pgsql/data -l logfile start

  SHELL
end
