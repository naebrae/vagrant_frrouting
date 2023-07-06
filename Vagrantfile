# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|

  config.vm.define :rtr0 do |rtr0_config|
    rtr0_config.vm.box = "boxomatic/alpine-3.18"
    rtr0_config.vm.provider "virtualbox" do |rtr0_vb|
      rtr0_vb.name = "rtr0"
      rtr0_vb.memory = 256
    end
    rtr0_config.vm.network "private_network", ip: "172.16.11.250"
    rtr0_config.vm.network "private_network", ip: "172.16.12.250"
    rtr0_config.vm.provision "file", source: "ssh.id_ecdsa", destination: "ssh.id_ecdsa"
    rtr0_config.vm.provision "file", source: "ssh.authorized_keys", destination: "ssh.authorized_keys"
    rtr0_config.vm.provision "file", source: "ospf/rtr0.ospfd.conf", destination: "ospfd.conf"
    rtr0_config.vm.provision "file", source: "ospf/rtr0.ospf6d.conf", destination: "ospf6d.conf"
    rtr0_config.vm.provision "shell", inline: <<-SHELL
      echo "net.ipv4.ip_forward = 1" >> /etc/sysctl.conf
      echo 1 > /proc/sys/net/ipv4/ip_forward
      echo "net.ipv6.conf.all.forwarding = 1" >> /etc/sysctl.conf
      echo 1 > /proc/sys/net/ipv6/conf/all/forwarding
      ip address add fd02:34bc:a5a8:8811::250/64 dev eth1
      ip address add fd02:34bc:a5a8:8812::250/64 dev eth2
      echo "rtr0.lab.home" > /etc/hostname && hostname `cat /etc/hostname`
      mkdir -p /root/.ssh && chmod 0700 /root/.ssh && cp /home/vagrant/ssh.id_ecdsa /root/.ssh/id_ecdsa && cp /home/vagrant/ssh.authorized_keys /root/.ssh/authorized_keys && chmod 0600 /root/.ssh/*
      cp /home/vagrant/ssh.id_ecdsa /home/vagrant/.ssh/id_ecdsa && cat /home/vagrant/ssh.authorized_keys >> /home/vagrant/.ssh/authorized_keys && chmod 0600 /home/vagrant/.ssh/* && chown vagrant:vagrant /home/vagrant/.ssh/*
      apk add frr shadow
      usermod -a -G frr,frrvty vagrant
      echo "hostname `hostname -s`" > /etc/frr/frr.conf && chown frr:frr /etc/frr/frr.conf
      cat /home/vagrant/ospfd.conf >> /etc/frr/frr.conf
      cat /home/vagrant/ospf6d.conf >> /etc/frr/frr.conf
      sed -i "s/ospfd=no/ospfd=yes/g" /etc/frr/daemons
      sed -i "s/ospf6d=no/ospf6d=yes/g" /etc/frr/daemons
      rc-update add frr default
      /etc/init.d/frr restart
    SHELL
  end

  config.vm.define :rtr1 do |rtr1_config|
    rtr1_config.vm.box = "boxomatic/alpine-3.18"
    rtr1_config.vm.provider "virtualbox" do |rtr1_vb|
      rtr1_vb.name = "rtr1"
      rtr1_vb.memory = 256
    end
    rtr1_config.vm.network "private_network", ip: "172.16.1.254"
    rtr1_config.vm.network "private_network", ip: "172.16.11.251"
    rtr1_config.vm.provision "file", source: "ssh.id_ecdsa", destination: "ssh.id_ecdsa"
    rtr1_config.vm.provision "file", source: "ssh.authorized_keys", destination: "ssh.authorized_keys"
    rtr1_config.vm.provision "file", source: "ospf/rtr1.ospfd.conf", destination: "ospfd.conf"
    rtr1_config.vm.provision "file", source: "ospf/rtr1.ospf6d.conf", destination: "ospf6d.conf"
    rtr1_config.vm.provision "shell", inline: <<-SHELL
      echo "net.ipv4.ip_forward = 1" >> /etc/sysctl.conf
      echo 1 > /proc/sys/net/ipv4/ip_forward
      echo "net.ipv6.conf.all.forwarding = 1" >> /etc/sysctl.conf
      echo 1 > /proc/sys/net/ipv6/conf/all/forwarding
      ip address add fd02:34bc:a5a8:8801::254/64 dev eth1
      ip address add fd02:34bc:a5a8:8811::251/64 dev eth2
      echo "rtr1.lab.home" > /etc/hostname && hostname `cat /etc/hostname`
      mkdir -p /root/.ssh && chmod 0700 /root/.ssh && cp /home/vagrant/ssh.id_ecdsa /root/.ssh/id_ecdsa && cp /home/vagrant/ssh.authorized_keys /root/.ssh/authorized_keys && chmod 0600 /root/.ssh/*
      cp /home/vagrant/ssh.id_ecdsa /home/vagrant/.ssh/id_ecdsa && cat /home/vagrant/ssh.authorized_keys >> /home/vagrant/.ssh/authorized_keys && chmod 0600 /home/vagrant/.ssh/* && chown vagrant:vagrant /home/vagrant/.ssh/*
      apk add frr shadow
      usermod -a -G frr,frrvty vagrant
      echo -e "hostname `hostname -s`" > /etc/frr/frr.conf && chown frr:frr /etc/frr/frr.conf
      cat /home/vagrant/ospfd.conf >> /etc/frr/frr.conf
      cat /home/vagrant/ospf6d.conf >> /etc/frr/frr.conf
      sed -i "s/ospfd=no/ospfd=yes/g" /etc/frr/daemons
      sed -i "s/ospf6d=no/ospf6d=yes/g" /etc/frr/daemons
      rc-update add frr default
      /etc/init.d/frr restart
    SHELL
  end

  config.vm.define :rtr2 do |rtr2_config|
    rtr2_config.vm.box = "boxomatic/alpine-3.18"
    rtr2_config.vm.provider "virtualbox" do |rtr2_vb|
      rtr2_vb.name = "rtr2"
      rtr2_vb.memory = 256
    end
    rtr2_config.vm.network "private_network", ip: "172.16.2.254"
    rtr2_config.vm.network "private_network", ip: "172.16.12.252"
    rtr2_config.vm.provision "file", source: "ssh.id_ecdsa", destination: "ssh.id_ecdsa"
    rtr2_config.vm.provision "file", source: "ssh.authorized_keys", destination: "ssh.authorized_keys"
    rtr2_config.vm.provision "file", source: "ospf/rtr2.ospfd.conf", destination: "ospfd.conf"
    rtr2_config.vm.provision "file", source: "ospf/rtr2.ospf6d.conf", destination: "ospf6d.conf"
    rtr2_config.vm.provision "shell", inline: <<-SHELL
      echo "net.ipv4.ip_forward = 1" >> /etc/sysctl.conf
      echo 1 > /proc/sys/net/ipv4/ip_forward
      echo "net.ipv6.conf.all.forwarding = 1" >> /etc/sysctl.conf
      echo 1 > /proc/sys/net/ipv6/conf/all/forwarding
      ip address add fd02:34bc:a5a8:8802::254/64 dev eth1
      ip address add fd02:34bc:a5a8:8812::252/64 dev eth2
      echo "rtr2.lab.home" > /etc/hostname && hostname `cat /etc/hostname`
      mkdir -p /root/.ssh && chmod 0700 /root/.ssh && cp /home/vagrant/ssh.id_ecdsa /root/.ssh/id_ecdsa && cp /home/vagrant/ssh.authorized_keys /root/.ssh/authorized_keys && chmod 0600 /root/.ssh/*
      cp /home/vagrant/ssh.id_ecdsa /home/vagrant/.ssh/id_ecdsa && cat /home/vagrant/ssh.authorized_keys >> /home/vagrant/.ssh/authorized_keys && chmod 0600 /home/vagrant/.ssh/* && chown vagrant:vagrant /home/vagrant/.ssh/*
      apk add frr shadow
      usermod -a -G frr,frrvty vagrant
      echo -e "hostname `hostname -s`" > /etc/frr/frr.conf && chown frr:frr /etc/frr/frr.conf
      cat /home/vagrant/ospfd.conf >> /etc/frr/frr.conf
      cat /home/vagrant/ospf6d.conf >> /etc/frr/frr.conf
      sed -i "s/ospfd=no/ospfd=yes/g" /etc/frr/daemons
      sed -i "s/ospf6d=no/ospf6d=yes/g" /etc/frr/daemons
      rc-update add frr default
      /etc/init.d/frr restart
    SHELL
  end

  config.vm.define :rh1101 do |rh1101_config|
    rh1101_config.vm.box = "almalinux/9"
    rh1101_config.vm.provider "virtualbox" do |rh1101_vb|
      rh1101_vb.name = "rh1101"
    end
    rh1101_config.vm.network "private_network", ip: "172.16.1.101"
    rh1101_config.vm.provision "file", source: "ssh.id_ecdsa", destination: "ssh.id_ecdsa"
    rh1101_config.vm.provision "file", source: "ssh.authorized_keys", destination: "ssh.authorized_keys"
    rh1101_config.vm.provision "shell", inline: <<-SHELL
      ip route add 172.16.2.0/24 via 172.16.1.254
      ip address add fd02:34bc:a5a8:8801::101/64 dev eth1
      ip -6 route add default via fd02:34bc:a5a8:8801::254
      echo "172.16.2.0/24 via 172.16.1.254" > /etc/sysconfig/network-scripts/route-eth1
      echo "172.16.1.101        rh1101.lab.home       rh1101" >> /etc/hosts
      echo "172.16.2.101        rh2101.lab.home       rh2101" >> /etc/hosts
      echo "172.16.2.102        rh2102.lab.home       rh2102" >> /etc/hosts
      echo "fd02:34bc:a5a8:8801::101        rh1101v6.lab.home       rh1101v6" >> /etc/hosts
      echo "fd02:34bc:a5a8:8802::101        rh2101v6.lab.home       rh2101v6" >> /etc/hosts
      echo "fd02:34bc:a5a8:8802::102        rh2102v6.lab.home       rh2102v6" >> /etc/hosts
      echo "rh1101.lab.home" > /etc/hostname && hostname `cat /etc/hostname`
      mkdir -p /root/.ssh && chmod 0700 /root/.ssh && cp /home/vagrant/ssh.id_ecdsa /root/.ssh/id_ecdsa && cp /home/vagrant/ssh.authorized_keys /root/.ssh/authorized_keys && chmod 0600 /root/.ssh/*
      cp /home/vagrant/ssh.id_ecdsa /home/vagrant/.ssh/id_ecdsa && cat /home/vagrant/ssh.authorized_keys >> /home/vagrant/.ssh/authorized_keys && chmod 0600 /home/vagrant/.ssh/* && chown vagrant:vagrant /home/vagrant/.ssh/*
    SHELL
  end

  config.vm.define :rh2101 do |rh2101_config|
    rh2101_config.vm.box = "almalinux/9"
    rh2101_config.vm.provider "virtualbox" do |rh2101_vb|
      rh2101_vb.name = "rh2101"
    end
    rh2101_config.vm.network "private_network", ip: "172.16.2.101"
    rh2101_config.vm.network "private_network", ip: "192.168.2.101", virtualbox__intnet: true
    rh2101_config.vm.provision "file", source: "ssh.id_ecdsa", destination: "ssh.id_ecdsa"
    rh2101_config.vm.provision "file", source: "ssh.authorized_keys", destination: "ssh.authorized_keys"
    rh2101_config.vm.provision "shell", inline: <<-SHELL
      ip route add 172.16.1.0/24 via 172.16.2.254
      ip address add fd02:34bc:a5a8:8802::101/64 dev eth1
      ip -6 route add default via fd02:34bc:a5a8:8802::254
      echo "172.16.1.0/24 via 172.16.2.254" > /etc/sysconfig/network-scripts/route-eth1
      echo "172.16.2.101        rh2101.lab.home       rh2101" >> /etc/hosts
      echo "172.16.2.102        rh2102.lab.home       rh2102" >> /etc/hosts
      echo "172.16.1.101        rh1101.lab.home       rh1101" >> /etc/hosts
      echo "fd02:34bc:a5a8:8801::101        rh1101v6.lab.home       rh1101v6" >> /etc/hosts
      echo "fd02:34bc:a5a8:8802::101        rh2101v6.lab.home       rh2101v6" >> /etc/hosts
      echo "fd02:34bc:a5a8:8802::102        rh2102v6.lab.home       rh2102v6" >> /etc/hosts
      echo "rh2101.lab.home" > /etc/hostname && hostname `cat /etc/hostname`
      mkdir -p /root/.ssh && chmod 0700 /root/.ssh && cp /home/vagrant/ssh.id_ecdsa /root/.ssh/id_ecdsa && cp /home/vagrant/ssh.authorized_keys /root/.ssh/authorized_keys && chmod 0600 /root/.ssh/*
      cp /home/vagrant/ssh.id_ecdsa /home/vagrant/.ssh/id_ecdsa && cat /home/vagrant/ssh.authorized_keys >> /home/vagrant/.ssh/authorized_keys && chmod 0600 /home/vagrant/.ssh/* && chown vagrant:vagrant /home/vagrant/.ssh/*
    SHELL
  end

  config.vm.define :rh2102 do |rh2102_config|
    rh2102_config.vm.box = "almalinux/9"
    rh2102_config.vm.provider "virtualbox" do |rh2102_vb|
      rh2102_vb.name = "rh2102"
    end
    rh2102_config.vm.network "private_network", ip: "172.16.2.102"
    rh2102_config.vm.network "private_network", ip: "192.168.2.102", virtualbox__intnet: true
    rh2102_config.vm.provision "file", source: "ssh.id_ecdsa", destination: "ssh.id_ecdsa"
    rh2102_config.vm.provision "file", source: "ssh.authorized_keys", destination: "ssh.authorized_keys"
    rh2102_config.vm.provision "shell", inline: <<-SHELL
      ip route add 172.16.1.0/24 via 172.16.2.254
      ip address add fd02:34bc:a5a8:8802::102/64 dev eth1
      ip -6 route add default via fd02:34bc:a5a8:8802::254
      echo "172.16.1.0/24 via 172.16.2.254" > /etc/sysconfig/network-scripts/route-eth1
      echo "172.16.2.102        rh2102.lab.home       rh2102" >> /etc/hosts
      echo "172.16.2.101        rh2101.lab.home       rh2101" >> /etc/hosts
      echo "172.16.1.101        rh1101.lab.home       rh1101" >> /etc/hosts
      echo "fd02:34bc:a5a8:8801::101        rh1101v6.lab.home       rh1101v6" >> /etc/hosts
      echo "fd02:34bc:a5a8:8802::101        rh2101v6.lab.home       rh2101v6" >> /etc/hosts
      echo "fd02:34bc:a5a8:8802::102        rh2102v6.lab.home       rh2102v6" >> /etc/hosts
      echo "rh2102.lab.home" > /etc/hostname && hostname `cat /etc/hostname`
      mkdir -p /root/.ssh && chmod 0700 /root/.ssh && cp /home/vagrant/ssh.id_ecdsa /root/.ssh/id_ecdsa && cp /home/vagrant/ssh.authorized_keys /root/.ssh/authorized_keys && chmod 0600 /root/.ssh/*
      cp /home/vagrant/ssh.id_ecdsa /home/vagrant/.ssh/id_ecdsa && cat /home/vagrant/ssh.authorized_keys >> /home/vagrant/.ssh/authorized_keys && chmod 0600 /home/vagrant/.ssh/* && chown vagrant:vagrant /home/vagrant/.ssh/*
    SHELL
  end

  config.vm.provision "shell", inline: <<-SHELL
    ln -f -s /usr/share/zoneinfo/Australia/Perth /etc/localtime
  SHELL
  config.vm.synced_folder ".", "/home/vagrant/sync", type: "rsync", disabled: true
  config.vm.synced_folder ".", "/vagrant", disabled: true
end
