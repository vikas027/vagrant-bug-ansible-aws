# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure('2') do |config|
config.vm.box                 = "aws-amazon-linux"
config.vm.box_url             = "https://github.com/mitchellh/vagrant-aws/raw/master/dummy.box"
config.vm.synced_folder '.', '/vagrant', type: "rsync", rsync__exclude: ".git/", owner: "root", group: "root"

  config.vm.provider :aws do |aws, override|
    override.ssh.private_key_path = 'my.pem'
    override.ssh.username         = 'ec2-user'
    override.ssh.forward_agent    = true

    aws.user_data                 = "#!/bin/bash\necho 'Defaults:ec2-user !requiretty' > /etc/sudoers.d/999-vagrant-cloud-init-requiretty && #!/bin/bash\necho 'Defaults:root !requiretty' >> /etc/sudoers.d/999-vagrant-cloud-init-requiretty && chmod 440 /etc/sudoers.d/999-vagrant-cloud-init-requiretty \n"
    aws.access_key_id             = ''
    aws.secret_access_key         = ''
    aws.keypair_name              = 'vagrant-test'
    aws.security_groups           = ["my-test"]
    aws.region                    = 'us-west-1'
    aws.ami                       = ''
    aws.instance_ready_timeout    = 180
    aws.instance_type             = 't2.micro'
    aws.elastic_ip                = false
  end

  config.vm.provision :shell, :privileged => false, :inline => "pwd && whoami && echo $PATH && ls -l /usr/local/bin/ansible*"
  config.vm.provision :ansible do |ansible|
    ansible.sudo = true
    ansible.verbose = true
    ansible.playbook = 'common.yml'
  end

end
