IMAGE_NAME = "k8s"
MASTER_IMAGE = "k8s-master"
MASTER_ADDR = "192.168.200.16"
WORKER_ADDR = "192.168.200.15"
BASE = "bento/ubuntu-16.04"

Vagrant.configure("2") do |config|

  config.vm.provider "virtualbox" do |v|
    v.memory = 2048
    v.cpus = 2
  end

  config.vm.define "k8s-master" do |master|
    master.vm.box = BASE
    master.vm.network "private_network", ip: MASTER_ADDR
    master.vm.hostname = "k8s-master"
    master.vm.synced_folder "./master/config", "/home/vagrant/share/config"
    master.vm.synced_folder "./master/token", "/home/vagrant/share/token"
    master.vm.synced_folder "./master/resource", "/home/vagrant/share/resource"

    master.vm.provision "shell" do |s|
      s.path = "./scripts/k8s_setup.sh"
    end

    master.vm.provision "shell" do |s|
      s.path = "./scripts/master_install.sh"
      s.args = [MASTER_ADDR, "10.10.0.0/16"]
    end
  end

  config.vm.define "k8s-worker" do |worker|
    worker.vm.box = IMAGE_NAME
    worker.vm.network "private_network", ip: WORKER_ADDR
    worker.vm.hostname = "k8s-worker"
    worker.vm.synced_folder "./master/config", "/home/vagrant/share/config"
    worker.vm.synced_folder "./master/token", "/home/vagrant/share/token"
    worker.vm.synced_folder "./labs", "/home/vagrant/share/lab"

    # worker.vm.provision "shell" do |s|
    #   s.path = "./scripts/k8s_setup.sh"
    # end

    worker.vm.provision "shell" do |s|
      s.inline = "sudo sh /home/vagrant/share/token/token.txt"
    end

    # worker.vm.provision "shell" do |s|
    #   s.inline = "export KUBECONFIG=/home/vagrant/share/config/admin.conf"
    # end
  end
end