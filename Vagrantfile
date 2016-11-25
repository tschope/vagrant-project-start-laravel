require 'yaml'

current_dir    = File.dirname(File.expand_path(__FILE__))
configs        = YAML.load_file("#{current_dir}/vagrant_config.yml")
vagrant_config = configs['configs']

def which(cmd)
    exts = ENV['PATHEXT'] ? ENV['PATHEXT'].split(';') : ['']
    ENV['PATH'].split(File::PATH_SEPARATOR).each do |path|
        exts.each { |ext|
            exe = File.join(path, "#{cmd}#{ext}")
            return exe if File.executable? exe
        }
    end
    return nil
end

Vagrant.configure("2") do |config|

    # Disable the default hostmanager behavior
    config.hostmanager.enabled = false
    ### ... possible provisioner config before hostmanager ...
    ### hostmanager provisioner
    config.vm.provision :hostmanager

    ### ... possible provisioning config after hostmanager ...
    config.hostmanager.enabled = true
    config.hostmanager.manage_host = true
    config.hostmanager.manage_guest = true
    config.hostmanager.ignore_private_ip = false
    config.hostmanager.include_offline = true
    config.vm.define vagrant_config['vm_name'] do |node|
      node.vm.hostname = vagrant_config['vm_url']
      node.vm.network :private_network, ip: vagrant_config['vm_ip']
    end

    config.vm.provider :virtualbox do |v|
        v.name = vagrant_config['vm_name']
        v.customize [
            "modifyvm", :id,
            "--name", vagrant_config['vm_name'],
            "--memory", vagrant_config['vm_memory'],
            "--natdnshostresolver1", "on",
            "--cpus", vagrant_config['vm_cpus'],
        ]
    end

    config.vm.box = vagrant_config['vm_box']

    config.vm.network :private_network, ip: vagrant_config['vm_ip']
    config.ssh.forward_agent = true

    # If ansible is in your path it will provision from your HOST machine
    # If ansible is not found in the path it will be instaled in the VM and provisioned from there
    if which('ansible-playbook')
        config.vm.provision "ansible" do |ansible|
            ansible.playbook = "ansible/playbook.yml"
            ansible.inventory_path = "ansible/inventories/dev"
            ansible.limit = 'all'
        end
    else
        config.vm.provision :shell, path: "ansible/windows.sh", args: [vagrant_config['vm_name']]
    end

    config.vm.synced_folder "./", vagrant_config['vm_path'], id: "vagrant-root", :owner => " www-data", :group => " www-data"

end
