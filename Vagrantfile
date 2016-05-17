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
    mkdir -p /var/www/default/html
    mv /vagrant/confd /var/confd
    mv /vagrant/index.html /var/www/default/html
  SHELL

  config.vm.provision :docker do |d|
    d.run "registrator",
      image: "gliderlabs/registrator",
      args: "--net host -v /var/run/docker.sock:/tmp/docker.sock",
      cmd: "-internal=true etcd://localhost:2379/haproxy-discover/services",
      restart: "always"
    d.run "haproxy-confd",
      image: "treehau5/haproxy-confd",
      args: "--net host -v /var/confd:/etc/confd -e 'ETCD_ADDR=localhost:2379' -p 80:80",
      restart: "always"
    d.run "www_1",
      image: "nginx",
      args: "-v /var/www/default/html:/usr/share/nginx/html",
      restart: "always"
  end
end
