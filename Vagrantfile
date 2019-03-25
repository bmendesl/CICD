# -*- mode: ruby -*-
# vi: set ft=ruby :

machines = {
	"scm" => {"memory"=>"4096", "cpus"=>"1", "ip" => "10" },
	"pipeline" => {"memory"=>"4096", "cpus"=>"1", "ip" => "20" },
	#"homolog" => {"memory"=>"2048", "cpus"=>"1", "ip" => "40" },
	#"production" => {"memory"=>"2048", "cpus"=>"1", "ip" => "50" },
}

Vagrant.configure("2") do |config|
  config.vm.box = "centos/7"
  
  machines.each do |name,conf|
    config.vm.define "#{name}" do |machine|
      machine.vm.network "private_network", ip: "192.168.88.#{conf["ip"]}"
      machine.vm.hostname = "#{name}.dexter.com.br"
      machine.vm.provider "virtualbox" do |vb|
        vb.cpus = "#{conf["cpus"]}"
	vb.memory = "#{conf["memory"]}"
	vb.name = "#{name}"
	vb.gui = false
      end
      
      if File.file?("provision/#{name}.sh")
	      config.vm.provision "shell", path: "provision/#{name}.sh"
      end 

    end
  end
  config.vm.provision "shell", inline: <<-SHELL
    yum install -y epel-release vim
  SHELL
end
