# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|
  config.vm.box = "dummy"

  config.vm.box_check_update = false

  config.vm.provider :aws do |aws, override|
    aws.access_key_id = "#{ENV['AWS_ACCESS_KEY_ID']}"
    aws.secret_access_key = "#{ENV['AWS_SECRET_ACCESS_KEY']}"
    aws.keypair_name = "#{ENV['AWS_KEYPAIR_NAME']}"

    aws.region = 'us-west-1'
    aws.instance_type = 't2.micro'
    
    aws.ami = 'ami-d27a03b2'
    aws.user_data = File.read ("user_data")
    aws.security_groups = ['coreos-nginx-server']

    override.ssh.username = 'core'
    override.ssh.private_key_path = "#{ENV['AWS_KEYPAIR_PATH']}"
  end

  config.vm.provision :shell, inline: <<-SHELL
    # configure motd
    mv /vagrant/motd /etc/motd
    chown root:root /etc/motd
    chmod 644 /etc/motd

    # configure login.defs
    mv /vagrant/login.defs /etc/login.defs
    chown root:root /etc/login.defs
    chmod 640 /etc/login.defs

    # configure confd for haproxy-confd container
    mv /vagrant/confd /var/confd
    chmod -R 644 /var/confd

    # configure html dir for nginx www server
    mkdir -p /var/www
    mv /vagrant/www/* /var/www
    chown -R core:core /var/www
    chmod -R 755 /var/www

    # hardening measures, remove coreos user from rkt group
    gpasswd -d core rkt

    # schedule www container and reverse proxy / load balancer for startup
    fleetctl start /opt/fleet/registrator.service
    fleetctl start /opt/fleet/haproxy-confd.service
    fleetctl start /opt/fleet/nginx.service
  SHELL

end
