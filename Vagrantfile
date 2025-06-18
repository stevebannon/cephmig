# -*- mode: ruby -*-
# vi: set ft=ruby :

servers=[
  {
    :hostname => "mon1-1",
    :box => "bento/ubuntu-22.04",
    :ram => 1024,
    :cpu => 1,
    :ip => "192.168.56.10",
    :priv_ip => "192.168.57.10"
  },
  {
    :hostname => "osd1-1",
    :box => "bento/ubuntu-22.04",
    :ram => 1024,
    :cpu => 1,
    :ip => "192.168.56.11",
    :priv_ip => "192.168.57.11"
  },
  {
    :hostname => "osd2-1",
    :box => "bento/ubuntu-22.04",
    :ram => 1024,
    :cpu => 1,
    :ip => "192.168.56.12",
    :priv_ip => "192.168.57.12"
  },
  {
    :hostname => "osd3-1",
    :box => "bento/ubuntu-22.04",
    :ram => 1024,
    :cpu => 1,
    :ip => "192.168.56.13",
    :priv_ip => "192.168.57.13"
  },
  {
    :hostname => "mon1-2",
    :box => "bento/ubuntu-22.04",
    :ram => 1024,
    :cpu => 1,
    :ip => "192.168.56.19",
    :priv_ip => "192.168.57.19"
  },
  {
    :hostname => "osd1-2",
    :box => "bento/ubuntu-22.04",
    :ram => 1024,
    :cpu => 1,
    :ip => "192.168.56.14",
    :priv_ip => "192.168.57.14"
  },
  {
    :hostname => "osd2-2",
    :box => "bento/ubuntu-22.04",
    :ram => 1024,
    :cpu => 1,
    :ip => "192.168.56.15",
    :priv_ip => "192.168.57.15"
  },
  {
    :hostname => "osd3-2",
    :box => "bento/ubuntu-22.04",
    :ram => 1024,
    :cpu => 1,
    :ip => "192.168.56.16",
    :priv_ip => "192.168.57.16"
  },
  {
    :hostname => "osd1-3",
    :box => "bento/ubuntu-22.04",
    :ram => 1024,
    :cpu => 1,
    :ip => "192.168.56.17",
    :priv_ip => "192.168.57.17"
  },
  {
    :hostname => "osd2-3",
    :box => "bento/ubuntu-22.04",
    :ram => 1024,
    :cpu => 1,
    :ip => "192.168.56.18",
    :priv_ip => "192.168.57.18"
  },
  {
    :hostname => "osd3-3",
    :box => "bento/ubuntu-22.04",
    :ram => 1024,
    :cpu => 1,
    :ip => "192.168.56.20",
    :priv_ip => "192.168.57.20"
  }
]

unless Vagrant.has_plugin?("vagrant-docker-compose")
    system("vagrant plugin install vagrant-docker-compose")
    puts "Compose plugin installed, please try the command again."
    exit
end

unless Vagrant.has_plugin?("vagrant-hostmanager")
    system("vagrant plugin install vagrant-hostmanager")
    puts "Hostmanager plugin installed, please try the command again."
    exit
end

Vagrant.configure("2") do |config|
    servers.each do |machine|
        config.hostmanager.enabled = true
        config.hostmanager.manage_host = true
        config.hostmanager.manage_guest = true
        config.hostmanager.ignore_private_ip = false
        config.hostmanager.include_offline = true
        config.vm.define machine[:hostname] do |node|
            node.vm.hostname = machine[:hostname]
            node.vm.box = machine[:box]
            node.vm.network "private_network", ip: machine[:ip], name: "vboxnet0"
            node.vm.network "private_network", ip: machine[:priv_ip], name: "vboxnet1"
            node.vm.provision :shell, inline: "echo 'ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDCb93BtJw8d6izc+kqIz1qsRxuxRULmV8Qy9Y2iMb8kTKvRbSli7RJ0lhR3/2WYvQewWvJntkUyfZ3BdYHvKAFzbfJYaKHOf/XJLMAZxrGrcb635ukV7dGtgsVPaAYwrXGFG1aqMOyv9IK9p7qZGLkhRF2IZejUhZqy2n18An5F8GE8toC9zRQ5i2vBJXl/3uIdtOyuw99Tq4p4Ob/+B9gjH8fFCBheaNbwVHzw5gg+FsvN4Nkj7LhmraEgvbrXsToZ3VW/R7gxf6Hytp7R1gAmVRYsCTcWKr3eTQprq2ZsE6pM5UA09s01Xw6AGLiXEMqlppWVCCBOtjlfa2jFOAz sbannon@eddie.el.nist.gov' > /root/.ssh/authorized_keys"
            node.vm.provision :shell, inline: "curl https://download.ceph.com/keys/release.asc | gpg --dearmor -o /etc/apt/trusted.gpg.d/cephadm.gpg"
            node.vm.provision :shell, inline: "echo 'deb https://download.ceph.com/debian-squid/ jammy main' > /etc/apt/sources.list.d/ceph.list"
            node.vm.provision :shell, inline: "apt-get update"
            node.vm.provision :docker
            node.vm.provider "virtualbox" do |vb|
                vb.memory = machine[:ram]
                vb.cpus = machine[:cpu]
            end
        end
    end
end

