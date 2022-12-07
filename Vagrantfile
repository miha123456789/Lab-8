
Vagrant.configure("2") do |config|
  config.vm.define "mmm8" do |mmm8|
    mmm8.vm.hostname = "mmm8"
    mmm8.vm.box = "centos/7"
    mmm8.vm.network "forwarded_port", guest: 80, host: 8888,host_ip: "127.0.0.1"
    mmm8.vm.network "forwarded_port", guest: 443, host: 8443, host_ip: "127.0.0.1"

    mmm8.vm.provider "virtualbox" do |vb|
      vb.name = "mmm8"
      vb.gui = false
      vb.memory = "1024"
    end
    
    mmm8.vm.provision "shell", run: "always",  inline: <<-SHELL
        echo "Hello from the Centos VM"
        setenforce 0
        sed -ie 's/^SELINUX=.*$/SELINUX=disabled/' /etc/selinux/config
        mkdir -p /var/www/website.com/html/
        mkdir -p /etc/httpd/conf.d/
        cp -rav /vagrant/www-content/* /var/www/website.com/html/
        cp -rav /vagrant/conf/* /etc/httpd/conf.d/
        mkdir /etc/ssl/privatekey
        chmod 700 /etc/ssl/privatekey
        openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/pki/tls/private/apache-selfsigned.key -out /etc/pki/tls/certs/apache-selfsigned.crt -subj "/C=UA/ST=Lvivska/L=Lviv/O=ITStep/OU=University/CN=127.0.0.1"
        yum install -y epel-release
        yum install -y httpd
        yum install -y mod_ssl
        httpd -t
        systemctl start httpd
        systemctl enable httpd
                
    SHELL
  end
  
  
end
