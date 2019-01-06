node {

stage 'Checkout Mastering OSA Repository'
   checkout scm

stage 'Preparing Ansible OpenStack'
   def folder = new File('/opt/openstack-ansible')
     if (!folder.exists()) {
        sh "sudo git clone https://openstack/openstack-ansible.git/opt/openstack-ansible"
        sh "sudo git checkout stable/pike ; sudo git checkout 16.0.23"
}

stage 'Bootstrapping Ansible'
sh "cd /opt/openstack-ansible ; sudo scripts/bootstrap-ansible.sh"

stage 'Bootstrapping Environment'
sh "cd /opt/openstack-ansible ; sudo scripts/bootstrap-aio.sh"

stage 'Settings Integrity AIO Check '
sh "cd /opt/openstack-ansible ; sudo /usr/local/bin/openstack-ansible etc/openstack_deploy/openstack_user_config.yml.aio --syntax-check"

stage 'Host Syntax Integrity Check'
sh "sudo /usr/local/bin/openstack-ansible /opt/openstack-ansible/playbooks/setup-hosts.yml --syntax-check"
stage 'Preparing Target Hosts'
sh "cd /opt/openstack-ansible/playbooks"
sh "sudo /usr/local/bin/openstack-ansible setup-hosts.yml"

stage 'Verifying Hosts Healthcheck'
sh "cd /opt/openstack-ansible/playbooks"
sh "sudo /usr/local/bin/openstack-ansible healthcheck-hosts.yml"

stage 'Infrastructure Syntax Integrity Check'
sh "sudo /usr/local/bin/openstack-ansible /opt/openstack-ansible/playbooks/setup-infrastructure.yml --syntax-check"

stage 'Installing Shared Services'
sh "cd /opt/openstack-ansible/playbooks"
sh "sudo /usr/local/bin/openstack-ansible setup-infrastructure.yml"

stage 'Verifying Infrastructure Healthcheck'
sh "cd /opt/openstack-ansible/playbooks"
sh "sudo /usr/local/bin/openstack-ansible healthcheck-infrastructure.yml"

stage 'OpenStack Syntax Integrity Check'
sh "sudo /usr/local/bin/openstack-ansible /opt/openstack-ansible/playbooks/setup-openstack.yml --syntax-check"

stage 'Installing OpenStack Services'
sh "cd /opt/openstack-ansible/playbooks"
sh "sudo /usr/local/bin/openstack-ansible setup-openstack.yml"

stage 'Verifying OpenStack Healthcheck'
sh "cd /opt/openstack-ansible/playbooks"
sh "sudo /usr/local/bin/openstack-ansible healthcheck-openstack.yml"
stage "Verifying deployment" {
def deploy = sh (script:"lxc-ls | grep utility &> /dev/null", returnStdout: true).trim()

try {
  if (deploy !=0)
      sh "exit 1"
   currentBuild.result = '[SUCCESS] OpenStack Environment Ready To Access'
}

 catch (Exception err) {
     currentBuild.result = '[FAILURE] Cannot find the utility container'
}
echo "Result: ${currentBuild.result}"
}
}


