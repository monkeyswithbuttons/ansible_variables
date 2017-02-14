machine_id = 1

Vagrant.configure("2") do |config|
    config.vm.box = 'debian/jessie64'

    N = 3
    (1..N).each do |machine_id|
        config.vm.define "machine#{machine_id}" do |machine|
            machine.vm.hostname = "machine#{machine_id}"
            machine.vm.synced_folder '.', "vagrant", disabled: true

            machine.vm.network "private_network", ip: "192.168.11.%i" % (10 + machine_id)
            machine.vm.network "private_network", ip: "192.168.12.%i" % (10 + machine_id)
            machine.vm.network "private_network", ip: "192.168.13.%i" % (10 + machine_id)

            # Only execute once the Ansible provisioner,
            # when all the machines are up and ready.
            if machine_id == N
                machine.vm.provision :ansible do |ansible|
                    # Disable default limit to connect to all the machines
                    ansible.limit = "all"
                    ansible.playbook = "playbook.yml"
                end
            end
        end
    end
end
