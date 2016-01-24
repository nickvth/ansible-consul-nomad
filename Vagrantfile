require 'yaml'

def load_customizations
  if !File.exist? "customizations"
    $stderr.puts "customizations not found"
    exit 1
  end
  YAML.load_file("customizations")
end
 
Vagrant.configure(2) do |config|
     # Setup virtual machine box. This VM configuration code is always executed.
    config.vm.box = "hfm4/centos7"

    config.vm.define "control1" do |control1|
        control1.vm.network "private_network", ip: "192.168.33.10"
        control1.vm.hostname = 'control1'
        control1.vm.provision "shell", inline:
          'echo "192.168.33.10 control1" > /etc/hosts'
    end

    config.vm.define "resource1" do |resource1|
        resource1.vm.network "private_network", ip: "192.168.33.11"
        resource1.vm.hostname = 'resource1'
        resource1.vm.provision "shell", inline:
          'echo "192.168.33.11 resource1" > /etc/hosts'

        resource1.vm.provision "ansible" do |ansible|
          ansible.playbook = "site.yml"
          ansible.verbose = 'v'
          ansible.extra_vars = load_customizations()
          ansible.host_key_checking = 'true'
          ansible.ask_sudo_pass = 'true'
          ansible.limit = 'all'
          ansible.groups = {
          "control_nodes" => ["control1"],
          "resource_nodes" => ["resource1"]
          }
        end
    end

    config.vm.provider :virtualbox do |v|
      v.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
      v.customize ["modifyvm", :id, "--memory", 1536]
      v.customize ["modifyvm", :id, "--cpus", 1]
      v.customize ["modifyvm", :id, "--ioapic", "on"]
      v.customize ["modifyvm", :id, "--rtcuseutc", "off"]
    end

    config.vm.provision "shell", inline:
      'sudo sed -ri "s/%admin ALL=NOPASSWD: ALL/%admin ALL=(ALL) NOPASSWD: ALL/" /etc/sudoers'
    
    config.vm.provision "shell", inline:
      'sudo yum -y update'

    config.ssh.forward_agent = true 

end
